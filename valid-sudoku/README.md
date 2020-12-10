# Valid Sudoku

Determine if a `9 x 9` Sudoku board is valid. Only the filled cells need to be
validated **according to the following rules**:

1. Each row must contain the digits `1-9` without repitition.
1. Each column must contain the digits `1-9` without repitition.
1. Each of the nine `3 x 3` sub-boxes of the grid must contain
   the digits `1-9` without repition.

**Note:**

* A Sudoku board (partially filled) could be valid but is not necessarily
  solvable.
* Only the filled cells need to be validated according to the mentioned rules.

## Hints

1. Think of what the reusable piece is here: check if these 9 numbers contain
   no duplicates.

## Solutions

### Valid with helpers

This solution extracts out a `valid` method to check an array of 9 characters
and helper methods to convert a row, col, or cube into an array of 9
characters. The check is done with a boolean array. If the same character is
seen twice, indicated by an index already showing `true`, the char array is not
valid.

```java
class Solution {
    public boolean isValidSudoku(char[][] board) {
        // Check the rows.
        for (int row = 0; row < board.length; row++) {
            if (!valid(row(row, board))) {
                return false;
            }
        }

        // Check the cols.
        for (int col = 0; col < board[0].length; col++) {
            if (!valid(col(col, board))) {
                return false;
            }
        }

        // Check the cubes.
        for (int row = 0; row < 7; row += 3) {
            for (int col = 0; col < 7; col += 3) {
                if (!valid(cube(row, col, board))) {
                    return false;
                }
            }
        }

        return true;
    }

    private char[] row(int row, char[][] board) {
        return board[row];
    }

    private char[] col(int col, char[][] board) {
        char[] result = new char[9];
        for (int row = 0; row < board.length; row++) {
            result[row] = board[row][col];
        }
        return result;
    }

    private char[] cube(int row, int col, char[][] board) {
        int fill = 0;
        char[] result = new char[9];
        for (int r = row; r < row + 3; r++) {
            for (int c = col; c < col + 3; c++) {
                result[fill++] = board[r][c];
            }
        }
        return result;
    }

    private boolean valid(char[] nums) {
        boolean[] check = new boolean[nums.length];
        for (int n : nums) {
            if (n == '.') {
                continue;
            }
            int idx = n - '1';
            if (check[idx]) {
                return false;
            } else {
                check[idx] = true;
            }
        }
        return true;
    }
}
```
