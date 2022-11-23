# Max Area of Island

You are given an `m x n` binary matrix `grid`. An island is a group of `1`'s
(representing land) connected **4-directionally** (horizontal or vertical).
You may assume all four edges of the grid are surrounded by water.

The **area** of an island is the number of cells with value `1` in the island.

Return the maximum **area** of an island in `grid`. If there is no island,
return 0.

## Hints

1. Solving this is the foundation for many matrix problems. Think of how you
   might mark where you have already visited while counting the island size.

## Solutions

### Expansion

I think this problem is likely a duplicate within leeter. I certainly remember
doing many like this before. Here we merely iterate through the grid calling
an expand routine to count the size of the island originating at each
`(m, n)`. The visited island cells are marked with `-1` to make sure we don't
visit them twice.

The time complexity of this solution is `O(m * n)` where `m` is the number of
rows and `n` is the number of columns. This comes from the nested loop and
possibly looking at each cell (to count it or to verify that it has already
been counted) a couple of times. The space complexity is `O(1)` as the space
allocated to solve is not dependant on the input size.

```java
class Solution {
    public int maxAreaOfIsland(int[][] grid) {
        int max = 0;
        for (int m = 0; m < grid.length; m++) {
            for (int n = 0; n < grid[0].length; n++) {
                int count = expand(grid, m, n);
                if (count > max) {
                    max = count;
                }
            }
        }
        return max;
    }

    private int expand(int[][] grid, int m, int n) {
        if (m < 0 || m >= grid.length || n < 0 || n >= grid[0].length || grid[m][n] != 1) {
            return 0;
        }
        grid[m][n] = -1;
        return 1 +
            expand(grid, m - 1, n) +
            expand(grid, m, n - 1) +
            expand(grid, m, n + 1) +
            expand(grid, m + 1, n);
    }
}
```
