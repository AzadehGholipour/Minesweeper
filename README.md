# Minesweeper

- [Introduction](#introduction)
- [Distinctiveness and Complexity](#distinctiveness-and-complexity)
- [Functions](#Functions)
- [How To Run](#how-to-run)
- [User Guide](#user-guide)

-----------------------------------------------------------------------------
## Introduction

**Minesweeper** is a puzzle game that consists of a grid of cells, where some of the cells contain hidden mines. Clicking on a cell that contains a `mine` detonates the mine, and causes the user to lose the game. Clicking on a `safe` cell (i.e., a cell that does not contain a mine) reveals a number that indicates how many neighboring cells – where a neighbor is a cell that is one square to the left, right, up, down, or diagonal from the given cell – contain a mine.
The goal of the game is to `flag` each of the mines. The player can flag a mine by right-clicking on a cell.

-----------------------------------------------------------------------------
## Distinctiveness and Complexity

Our goal in this project will be to build an AI that can play Minesweeper. Recall that **knowledge-based agents** make decisions by considering their knowledge base, and making inferences based on that knowledge.

The way we could represent the AI’s knowledge about a Minesweeper game is: 
- by making each cell a propositional variable that is `true` if the cell contains a     mine, and `false` otherwise.
- Every logical sentence has two parts: a set of cells on the board that are involved in the sentence, and a number count, representing the count of how many of those cells are mines.
    - any time the number of cells is equal to the count, we know that all of that sentence’s cells must be mines.
    - any time we have a sentence whose count is 0, we know that all of that sentence’s cells must be safe.
    - any time we have two sentences set1 = count1 and set2 = count2 where set1 is a subset of set2, then we can construct the new sentence set2 - set1 = count2 - count1. 

There are two main files in this project:
- Runner.py has been implemented to run the graphical interface for the game.
- In `minesweeper.py` file, there are three classes defined:
    - Minesweeper, which handles the gameplay. each cell is a pair (i, j) where i is the row number (ranging from 0 to height - 1) and j is the column number (ranging from 0 to width - 1).
    - Sentence, which represents a logical sentence that contains both a set of cells and a count. 
    - MinesweeperAI, which handles inferring which moves to make based on knowledge.

-----------------------------------------------------------------------------
## Functions

- There are functions:
    - `known_mines` returns all the cells of a set as mine, if the length of the set equals to the `count`.
    - `known_safes` returns all the cells of a set as safe, if the length of the set equals to `zero`.
    - `mark_mine` removes a certain mine cell from a sentence and reduces one number from the `count`.
    - `mark_safe` removes a certain safe cell from a sentence but doesn't reduce the `count`.
    - `add_knowledge`:
        - Accepts a cell as a tuple `(i, j)` and its corresponding `count`.
        - Using a `double loop` over `(i, j)`, creates a set of 8 neighboring cells, counts how many neighbors are already known mines.
        - Adds unknown neighbors to the set (not in `self.safes`, `self.mines`, or `self.moves_made`).
        - Calculates remaining unknown mines in neighbors: neighbor_mines = count - mine_counter.
        - Creates a new `sentence` with the unknown neighbors and `neighbor_mines`.
        - Adds the new `sentence` to self.knowledge.
        - Iterates through all sentences to mark cells that must be mines or safe.
        - Removes empty sentences.
        - If len(cells) == count, all remaining cells are mines, marks them as safe.
        - If count == 0, all remaining cells are safe, marks them as safe.
        - According to the `subset method`, uses a `nested for loop` to compare every unique pair of sentences in `self.knowlwdge`, if they are subset, it updates the larger sentence by subtracting the subset’s cells/count.
    - `make_safe_move`:
        Iterates through all cells in `self.safes` using a for loop, checks if the cell is not in `self.moves_made` (has not been played yet), the function immediately returns the cell (prioritizing early termination). If no safe moves remain (all safe cells are already played), returns `None`.
    - `make_random_move`:
        - Selects a random valid cell to play when no safe moves are known.
        - Makes a set of all possible choices. every choice is a tuple `(i, j)` on the board which made by a `nested for loop` (i from `self.height` and j from `self.width`).
        - Checks if `(i, j)` is not in `self.mines` and not in `self.moves_made` (the choice is not played and not mine), the tuple can be appended to the `possible_moves` set.
        - If possible_moves is non-empty, returns a random choice from the `possible_moves` set. If there is no item in the set (no any possible choice), returns `None`. 

-----------------------------------------------------------------------------
## How To Run

Follow these steps to set up and run the degrees.py:

- Navigate to the project directory: /workspaces/Minesweeper
- Run `pip3 install -r requirements.txt` to install the required Python package `pygame`
- Run `python runner.py`
- Play Minesweeper or let your AI play for you!

-----------------------------------------------------------------------------
## User Guide

- After running the game you can choose any of the cells on the board or let AI choose some moves by clicking **AI Move** button.
- You can flag a cell by clicking the right button on the mouse if you recognize it is mine.
