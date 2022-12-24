# Number of Closed Islands

Given a 2d `grid` consisting of `0s` (land) and `1s` (water), an island is a
maximal 4-directionally connected group of `0s` and a closed island **totally**
(all left, top, right, bottom) surrounded by `1s`. Return the number of closed
islands.

## Hints

1. Kind of a standard grid expansion problem.

## Solutions

### Are you closed brah?

As mentioned in the hint this is pretty much a standard grid expansion problem.
The only difference here is that instead of perhaps needing to know the count
of islands or size of each island, we need to know the count of **closed**
islands. Thus we modify a standard expansion to return two characteristics of
a cell. First, if it is connected to an island, and second if it is closed.

The time complexity of this solution is O(m * n) where `m` is the number of
rows and `n` is the number of columns. This comes from iterating the grid,
perhaps up to twice as we iterate and mark islands. Since we are using recursion
here the space complexity is the deepest we may have to go in recursive calls.
This is O(max(m, n)) as we expand across the largest expanse of the grid.

```java
class Solution {
    private class IslandResult {
        boolean island;
        boolean closed;
        IslandResult(boolean island, boolean closed) {
            this.island = island;
            this.closed = closed;
        }
    }

    public int closedIsland(int[][] grid) {
        int closed = 0;
        for (int m = 0; m < grid.length; m++) {
            for (int n = 0; n < grid[0].length; n++) {
                IslandResult ir = new IslandResult(false, true);
                expand(m, n, grid, ir);
                if (ir.island && ir.closed) {
                    closed++;
                }
            }
        }

        return closed;
    }

    private void expand(int m, int n, int[][] grid, IslandResult ir) {
        if (m < 0 || m >= grid.length || n < 0 || n >= grid[0].length) {
            ir.closed = false;
            return;
        }

        if (grid[m][n] == 0) {
            ir.island = true;
            grid[m][n] = 1;
            expand(m - 1, n, grid, ir);
            expand(m, n - 1, grid, ir);
            expand(m, n + 1, grid, ir);
            expand(m + 1, n, grid, ir);
        }
    }
}
```
