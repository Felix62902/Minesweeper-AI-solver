# Minesweeper AI Solver

This project features a sophisticated AI agent capable of playing and solving the classic game of Minesweeper. The AI does not rely on brute force or simple heuristics; instead, it uses a knowledge base and a logical inference engine to make guaranteed safe moves. When no safe move can be determined, it makes a calculated random guess.

This project was completed as part of Harvard's CS50 AI course.

## How It Works: The AI's Logic

The AI operates as a **knowledge-based agent**. It maintains a collection of logical sentences about the state of the game board and uses this "knowledge" to deduce which cells are safe and which contain mines.

### 1. Knowledge Representation

Instead of using complex propositional logic sentences, which would be computationally expensive, the AI uses a custom `Sentence` structure. Each sentence consists of:
1.  A **set of cells** that are currently unknown.
2.  A **count** representing how many of those cells are mines.

For example, the sentence `{ (0,1), (0,2), (1,1) } = 2` means that out of the three specified cells, exactly two of them are mines.

### 2. The Inference Engine

The core of the AI is its `add_knowledge()` method, which functions as an inference engine. When new information is received (i.e., a safe cell is clicked and its neighbor count is revealed), the AI performs a chain reaction of deductions:

1.  **New Information:** A new sentence is added to the AI's knowledge base from the revealed cell. For example, clicking a cell with a "1" next to two unknown neighbors `A` and `B` generates the sentence `{A, B} = 1`.

2.  **Identifying Knowns:** The AI iterates through its knowledge base, looking for trivial conclusions.
    * If a sentence is `S = n` where the number of cells in `S` is also `n`, all cells in `S` are marked as **mines**.
    * If a sentence is `S = 0`, all cells in `S` are marked as **safe**.

3.  **Propagation:** When a new mine or safe cell is identified, this fact is propagated through the entire knowledge base. Sentences are simplified by removing the known cell and, in the case of a mine, decrementing the count.

4.  **Subset Inference:** The most powerful deduction comes from the subset rule. If the AI knows two sentences, `S1 = c1` and `S2 = c2`, and `S2` is a subset of `S1`, it can infer a new sentence:
    `S1 - S2 = c1 - c2`
    For example, from `{A, B, C} = 2` and `{A, B} = 1`, the AI can infer `{C} = 1`, concluding that `C` is a mine.

This process of identifying knowns, propagating facts, and inferring new sentences repeats in a loop until no new deductions can be made from the current knowledge.

### 3. Making a Move

-   **Safe Move:** The AI will always prioritize making a move on a cell that it has proven to be safe. It consults its set of known safe cells and chooses one that has not yet been played.
-   **Random Move:** If no guaranteed safe moves are available, the AI will make a random move, intelligently choosing only from cells that are not known mines and have not already been played.

## Project Structure

-   **`runner.py`**: The main graphical interface for the game, built using `pygame`. This file handles rendering the board and player/AI interactions. *(Provided by CS50)*.
-   **`minesweeper.py`**: Contains all the core logic for the project, including:
    -   `Minesweeper()`: Class representing the game board and its rules.
    -   `Sentence()`: Class representing a single logical sentence about the game state.
    -   `MinesweeperAI()`: The implementation of the intelligent agent, containing the knowledge base and inference logic.

## How to Run

1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/YOUR_USERNAME/YOUR_REPOSITORY_NAME.git](https://github.com/YOUR_USERNAME/YOUR_REPOSITORY_NAME.git)
    cd YOUR_REPOSITORY_NAME
    ```

2.  **Install dependencies:**
    Make sure you have Python 3 installed. Then, install the required `pygame` library.
    ```bash
    pip install -r requirements.txt
    ```

3.  **Run the game:**
    ```bash
    python runner.py
    ```
    You can play the game yourself, or click the "AI Move" button to let the AI take over.

## Acknowledgments
- This project is based on the "Minesweeper" project from the [CS50's Introduction to Artificial Intelligence with Python](https://cs50.harvard.edu/ai/2020/) course.
- The GUI and game engine (`runner.py`) were provided as part of the course's distribution code.
