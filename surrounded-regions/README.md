# Surrounded Regions

Given a 2D board containing `'X'` and `'O'` (**the letter O**), capture all
regions surrounded by `'X'`.

A region is captured by flipping all `'O'`s into `'X'`s in that surrounded
region.

Example:

```
X X X X
X O O X
X X O X
X O X X
```

After running your function, the board should be:

```
X X X X
X X X X
X X X X
X O X X
```

**Explanation:**

Surrounded regions shouldn't be on the border, which means that any `'O'` on
border of the board are not flipped to `'X'`. Any `'O'` that is not on the
border and it is not connected to an `'O'` on the border will be flipped to
`'X'`. Two cells are connected if they are adjacent cells connected
horizontally or vertically.

## Hints

1. The key to doing this problem simply is to read the description carefully.
   A straight up flipping of the "surrounded" cells to `'X'` is tricky. It is
   easier to think in terms of the special nature of the border `'O'`s and the
   other `'O'`s connected to them.

## Solutions

### Use the border

This solution calls a recursive `expandMark` method on each border cell.
These cells, if they contain `'O'`, are marked with a `'.'` to indicate
they should continue to be `'O'` after a second pass. `expandMark` marks
all cells connected to such a border cell similarly.

```java
class Solution {
    public void solve(char[][] board) {
        int m = board.length;
        if (m == 0) {
            return;
        }
        int n = board[0].length;

        // Expand mark the 'O' in the first and last row. These are 'O'
        // that should not be flipped.
        for (int row = 0; row < m; row++) {
            expandMark(row, 0, board);
            expandMark(row, n - 1, board);
        }

        // Expand mark the 'O' in the first and last column. Note that we
        // can skip those already hit by the row iteration above.
        for (int col = 1; col < n - 1; col++) {
            expandMark(0, col, board);
            expandMark(m - 1, col, board);
        }

        // Now everything still marked with 'O' is surrounded and should become 'X'.
        // Those marked with '.' should go back to 'O' as they are edge connected.
        for (int row = 0; row < m; row++) {
            for (int col = 0; col < n; col++) {
                switch (board[row][col]) {
                  case 'O':
                    board[row][col] = 'X';
                    break;
                  case '.':
                    board[row][col] = 'O';
                    break;
                }
            }
        }
    }

    private void expandMark(int row, int col, char[][] board) {
        if (row < 0 || row >= board.length ||
            col < 0 || col >= board[0].length) {
            // Out of bounds.
            return;
        }
        if (board[row][col] == 'O') {
            // Mark as should be 'O' after second pass.
            board[row][col] = '.';
            expandMark(row - 1, col, board);
            expandMark(row + 1, col, board);
            expandMark(row, col - 1, board);
            expandMark(row, col + 1, board);
        }
    }
}
```
