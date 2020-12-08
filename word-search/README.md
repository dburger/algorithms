# Word Search

Given an `m x n board` and a `word`, find if the word exists in the grid.

The word can be constructed from letters of sequentially adjacent cells, where
"adjacent" cells are horizontally or vertically neighboring (not diagonally).
The same letter cell may not be used more than once.

## Hints

1. The basic brute forcer would iterate through every cell and attempt to expand
   from there.

## Solutions

### Brute force expansion

This brute force approach merely tries to expand from each cell using a
recursive algorithm. Note the marking of a cell as "visited" by replacing its
content with a '.' when attempting to expand a solution.

```java
class Solution {
    public boolean exist(char[][] board, String word) {
        for (int row = 0; row < board.length; row++) {
            for (int col = 0; col < board[0].length; col++) {
                if (expands(row, col, board, word)) {
                    return true;
                }
            }
        }
        return false;
    }

    private boolean expands(int row, int col, char[][] board, String word) {
        if (word.isEmpty()) {
            return true;
        }
        if (row < 0 || row >= board.length || col < 0 || col >= board[0].length) {
            return false;
        }
        char c = word.charAt(0);
        if (board[row][col] != c) {
            return false;
        }
        // This effectively marks a cell as "visited", so that a solution
        // won't attempt to use the same cell twice.
        board[row][col] = '.';
        String part = word.substring(1);
        boolean expands = expands(row - 1, col, board, part) ||
            expands(row + 1, col, board, part) ||
            expands(row, col - 1, board, part) ||
            expands(row, col + 1, board, part);
        board[row][col] = c;
        return expands;
    }
}
```
