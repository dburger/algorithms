# Game of Life

The board is made up of an `m x n` grid of cells, where each cell has an
initial state: **live** (represented by a `1`) or **dead** (represented by a
`0`). Each cell interacts with its **eight neighbors** (horizontal, vertical,
diagonal) using the following four rules:

1. Any live cell with fewer than two live neighbors dies as if caused by
   under-population.
1. Any live cell with two or three live neighbors lives on to the next
   generation.
1. Any live cell with more than three live neighbors dies, as if by
   over-population.
1. Any dead cell with exactly three live neighbors becomes a live cell, as if
   by reproduction.

The next state is created by applying the above rules simultaneously to every
cell in the current state, where births and deaths occur simultaneously. Given
the current state of the `m x n` grid `board`, return the next state.

## Hints

1. Straightforward should work, although it will take an extra matrix to do it.

## Solutions

### Count and set

Here we use the most straightforward solution possible. Count for each cell,
and copy the new state into a temp matrix. When finished, copy the temp matrix
values back into the source matrix.

The time complexity of this algorith is O(m x n) where `m` is the number of
rows and `n` is the number of columns. This comes from visiting each cell and
doing the counts for each cell. The space complexity is also O(m x n) to store
the temporary matrix.

Could we do better? Time complexity wise no. We have to visit each cell which
is already O(m x n). Space complexity wise we could do better by doing the
algorithm in place. This would require an encoding scheme indicate what the
previous value was and what it should be changed to.

```java
class Solution {
    private final int[][] offsets = {
        {-1, -1},
        {-1, 0},
        {-1, 1},
        {0, -1},
        {0, 1},
        {1, -1},
        {1, 0},
        {1, 1}
    };

    public void gameOfLife(int[][] board) {
        int[][] temp = new int[board.length][board[0].length];
        for (int m = 0; m < board.length; m++) {
            for (int n = 0; n < board[0].length; n++) {
                int count = count(board, m, n);
                int val = board[m][n];
                if (val == 1) {
                    if (count < 2 || count > 3) {
                        val = 0;
                    }
                } else if (count == 3) {
                    val = 1;
                }
                temp[m][n] = val;
            }
        }

        for (int m = 0; m < board.length; m++) {
            for (int n = 0; n < board[0].length; n++) {
                board[m][n] = temp[m][n];
            }
        }
    }

    private int count(int[][] board, int m, int n) {
        int count = 0;
        for (int[] offset : offsets) {
            int row = m + offset[0];
            int col = n + offset[1];
            if (row > -1 && col > -1 && row < board.length && col < board[0].length && board[row][col] == 1) {
                count++;
            }
        }
        return count;
    }
}
```
