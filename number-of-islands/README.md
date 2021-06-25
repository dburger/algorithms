# Number of Islands

Given an `m x n` 2D binary grid `grid` which represents a map of `'1'`s (land) and `'0'`s
(water), return *the number of islands*.

An **island** is surrounded by water and is formed by connecting adjacent lands horizontally or
vertically. You may assume all four edges of the grid are all surrounded by water.

## Hints

1. Islands can be marked while iterating over the grid.

## Solutions

### Mark and go

This solution uses a standard technique of marking islands with a character other than `'1'` and
`'0'` while keeping track of the number of islands encountered. Here we start with `'a'` and
give each island encountered the next character.

The time complexity for this solution is O(m x n), the size of the grid. There is an obvious
iteration through this grid, and the `mark` function can explore into each cell a second time.
So the time complexity comes from a bounded multiple of the grid size. The space complexity for
this solution is O(1) as the grid is marked up in place.

```java
class Solution {
    public int numIslands(char[][] grid) {
        int islands = 0;
        for (int r = 0; r < grid.length; r++) {
            for (int c = 0; c < grid[0].length; c++) {
                if (mark(r, c, grid, (char) ('a' + islands)) > 0) {
                    islands++;
                }
            }
        }
        return islands;
    }

    // Returns the number of cells marked for the island or 0 if no island.
    private static int mark(int r, int c, char[][] grid, char marker) {
        if (r < 0 || r >= grid.length || c < 0 || c >= grid[0].length) {
            return 0;
        }
        if (grid[r][c] != '1') {
            return 0;
        }
        grid[r][c] = marker;
        return 1 +
            mark(r - 1, c, grid, marker) +
            mark(r + 1, c, grid, marker) +
            mark(r, c - 1, grid, marker) +
            mark(r, c + 1, grid, marker);
    }
}
```
