---
layout: post
title: Python COMP112 - tic tac toe
categories:
- assignment
tags:
- Python
- Data Structure
- turtle
---

## Tic Tac Toe 
You will implement a version of the classic “tic-tac-toe” game, in which the player 
competes against the computer to create a three-in-a-row pattern in vertical, horizontal, 
or diagonal directions. 

The game is played in a 3-by-3 grid, which is initially empty. 
There are two players, the human and the computer, who take turns. 
The human always moves first. When it’s the human’s turn, 
he or she may place an “O” symbol in any empty position on the game grid 
by clicking on the grid position with the mouse; afterwards, it becomes 
the computer’s move, and the computer will place a “X” symbol in any empty position on the grid. 
The players continue to alternate moves until the game ends. 
The game is over when any of the following conditions occurs: 

there are three “O” symbols in a line in any row or column, or diagonally from corner to corner, which means that the human has won 
there are three “X” symbols in a line in any row or column, or diagonally from corner to corner, which means that the computer has won 
there are no empty positions on the board, meaning that no further moves are possible, and the game ends in a stalemate 

As part of this project, you must develop a reasonable (but simple) 
“artificial intelligence” to dictate the behavior of the computer player. 
Although you may provide a more advanced algorithm (which your instructor 
will help you develop, if you ask), at a minimum, you must implement the 
following logic for the computer player: 

If it’s possible for the computer player to win in the current move 
(i.e. there are two “X”s and one empty position in a line somewhere on the board), 
the computer player must win by occupying that empty position. 
If it’s possible for the human player to win on the next move 
(i.e. there are two “O”s and one empty position in a line somewhere on the board), 
the computer player must prevent the human’s win by occupying that empty position. 
If neither of the above cases apply, the computer may randomly occupy any unoccupied position.

Getting started We provide you with starter code to help you begin this project. 
See the file in starter.zip. This is a starting point; 
you should replace the pass lines with code to perform the appropriate actions. 
