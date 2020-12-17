# Minesweeper

Let's play [minesweeper](https://en.wikipedia.org/wiki/Minesweeper_(video_game)).

You are given a 2D char matrix representing the game board. **'M'** represents
an **unrevealed** min, **'E'** represents an **unrevealed** empty square,
**'B'** represents a **revealed** blank square that has no adjacent (above,
below, left, right, and all 4 diagonals) mines, **digit** ('1' to '8')
represents how many mines are adjacent to this **revealed** square, and finally
**'X'** represents a **revealed** mine.

Now given the next click position (row and column indices) among all the
**unrevealed** squares ('M' or 'E'), return the board after revealing this
position according to the following rules:

1. If a mine ('M') is revealed, then the game is over- change it to **'X'**.
1. If an empty square ('E') with **no adjacent mines** is revealed, then change
   it to revealed blank ('B') and all of its adjacent **unrevealed** squares
   should be revealed recursively.
1. If an empty square ('E') with **at least one adjacent mine** is revealed,
   then change it to a digit ('1' to '8') representing the number of adjacent
   mines.
4. Return the board when no more squares will be revealed.

## Hints

1. This problem has a core common to some matrix problems, an "expand" method
   that recursively operates on the cells of the matrix. Other than that, this
   is all about handling the details.

## Solutions

### Expand attack

This is the typical recursive expand approach.

```java
class Solution {
    public char[][] updateBoard(char[][] board, int[] click) {
        int row = click[0];
        int col = click[1];
        if (board[row][col] == 'M') {
            board[row][col] = 'X';
        } else {
            int mines = mines(board, row, col);
            if (mines > 0) {
                board[row][col] = Character.forDigit(mines, 10);
            } else {
                expand(board, row, col);
            }
        }        
        return board;
    }

    private boolean valid(char[][] board, int row, int col) {
        return row >= 0 && row < board.length && col >= 0 && col < board[0].length;
    }

    private int mines(char[][] board, int row, int col) {
        int count = 0;
        if (!valid(board, row, col)) {
            return count;
        }
        for (int r = row - 1; r <= row + 1; r++) {
            for (int c = col - 1; c <= col + 1; c++) {
                if (r == row && c == col || !valid(board, r, c)) continue;
                if (valid(board, r, c) && board[r][c] == 'M') {
                    count++;
                }
            }
        }
        return count;
    }

    private void expand(char[][] board, int row, int col) {
        if (!valid(board, row, col) || board[row][col] != 'E') {
            return;
        }
        int mines = mines(board, row, col);
        if (mines > 0) {
            board[row][col] = Character.forDigit(mines, 10);
        } else {
            board[row][col] = 'B';
            for (int r = row - 1; r <= row + 1; r++) {
                for (int c = col - 1; c <= col + 1; c++) {
                    if (r == row && c == col || !valid(board, r, c)) continue;
                    expand(board, r, c);
                }
            }
        }
    }
}
```
