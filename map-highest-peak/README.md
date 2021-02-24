# Map of Highest Peak

You are given an integer matrix `water` of size `m x n` that represents a map of
land and water cells.

* if `water[i][j] == 0`, cell `(i, j)` is a land cell
* if `water[i][j] == 1`, cell `(i, j)` is a water cell

You must assign each cell a height in a way that follows these rules:

* The height of each cell must be non-negative.
* If the cell is a water cell, its height must be `0`.
* Any two adjacent cells must have an absolute height difference of at most `1`.
  A cell is adjacent to another cell if the former is directly north, east,
  south, or west of the latter (i.e., their sides are touching).

Find an assignment of heights such that the maximum height in the matrix is
maximized.

Return an integer matrix `height` of size `m x n` where `height[i][j]` is cell
`(i, j)`'s height. If there are multiple solutions, return any of them.

## Hints

1. Rather clever problem. Note that for any cell its height is bound by the
   closest water cell. That is it can only go up one in value for each hop
   from that closest water cell.

## Solutions

### BFS for the win

This problem is nicely solved with a breadth first search (BFS) approach. Per
the hint above, if you enter the water cells in a queue and then process the
queue in a BFS fashion, then you will be setting the cells height values per
the closest water cell. This is exactly what you need to do.

The space complexity of this solution is O(m x n) where m is the number of rows
and n is the number of columns. This is because a queue is made that holds cells
that need to be processed, starting with the water cells. The number of cells
that this queue could contain is bound by the size of the matrix. The time
complexity is also O(m x n). Each cell in the matrix will be visited first to
do the initializing of the matrix, and later to set each cell's value.

```java
class Solution {
    private class Cell {
        final int row;
        final int col;

        Cell(int row, int col) {
            this.row = row;
            this.col = col;
        }
    }

    public int[][] highestPeak(int[][] water) {
        int m = water.length;
        int n = water[0].length;

        Queue<Cell> q = new LinkedList<>();

        // Iterate through the table, marking the water with 0, everything
        // else with -1. Also add the water cells to the queue.
        for (int row = 0; row < m; row++) {
            for (int col = 0; col < n; col++) {
                if (water[row][col] == 1) {
                    water[row][col] = 0;
                    q.add(new Cell(row, col));
                } else {
                    water[row][col] = -1;
                }
            }
        }

        // Until queue is empty remove cell, attempt to set its four neighbors if they
        // 1. are not water, and 2. have not already been set.
        while (!q.isEmpty()) {
            Cell c = q.remove();
            // The neighbor, if not already set, has value of this cell plus one.
            int val = water[c.row][c.col] + 1;
            if (set(water, val, c.row - 1, c.col, m, n)) q.add(new Cell(c.row - 1, c.col));
            if (set(water, val, c.row + 1, c.col, m, n)) q.add(new Cell(c.row + 1, c.col));
            if (set(water, val, c.row, c.col - 1, m, n)) q.add(new Cell(c.row, c.col - 1));
            if (set(water, val, c.row, c.col + 1, m, n)) q.add(new Cell(c.row, c.col + 1));
        }

        return water;
    }

    private boolean set(int[][] water, int val, int row, int col, int m, int n) {
        if (row < 0 || col < 0 || row >= m || col >= n) {
            // Out of bounds.
            return false;
        }
        if (water[row][col] != -1) {
            // Already been set.
            return false;
        }
        // Not already set, set the value and indicate this cell should also be processed
        // in the queue.
        water[row][col] = val;
        return true;
    }
}
```
