# Making a Large Island

In a 2D grid of `0`s and `1`s, we change at most one `0` to a `1`.

After, what is the size of thee largest island? (An island is a 4-directionally
connected group of `1`s).

## Hints

1. If you could determine the size of each existing island, with no thought of
   changing any `0` to a `1`, then a second pass could consider changing `0`s
   to `1`s and add up the counts for the separate islands that it touches.

## Solutions

### Mark and go

This solution follows exactly what the hint lays out. A first pass merely counts
the existing islands and records the counts in a map. The `1`s of an island are
changed to and `id` identifier to avoid double counting. Then a second pass
through the grid looks for zeroes and sums up the 4-directionally connected
island counts.

```java
class Solution {
    public int largestIsland(int[][] grid) {
        Map<Integer, Integer> islands = new HashMap<>();
        // Seed id 0 with 0. When calculating a count this will take
        // care of cells with 0s in them and cells that are out of bounds.
        islands.put(0, 0);

        // Identify the islands, mark them, and record their count.
        int id = 2;
        for (int r = 0; r < grid.length; r++) {
            for (int c = 0; c < grid[0].length; c++) {
                int result = mark(grid, r, c, id);
                islands.put(id, result);
                id++;
            }
        }

        // Now look for the 0 that in flipping to 1, would create the
        // largest island.
        int max = 0;
        for (int r = 0; r < grid.length; r++) {
            for (int c = 0; c < grid.length; c++) {
                if (grid[r][c] == 0) {
                    max = Math.max(max, count(grid, r, c, islands));
                }
            }
        }

        // If max is 0 then no zeroes were found, and the entire grid
        // is already 1s.
        return max == 0 ? grid.length * grid[0].length : max;
    }

    private int mark(int[][] grid, int r, int c, int id) {
        if (r < 0 || r >= grid.length || c < 0 || c >= grid[0].length) {
            return 0;
        }
        if (grid[r][c] != 1) {
            return 0;
        }
        // Mark it to avoid double count.
        grid[r][c] = id;
        return 1 +
            mark(grid, r - 1, c, id) +
            mark(grid, r + 1, c, id) +
            mark(grid, r, c - 1, id) +
            mark(grid, r, c + 1, id);
    }

    private int count(int[][] grid, int r, int c, Map<Integer, Integer> islands) {
        Set<Integer> ids = new HashSet<>();
        ids.add(id(grid, r + 1, c));
        ids.add(id(grid, r - 1, c));
        ids.add(id(grid, r, c - 1));
        ids.add(id(grid, r, c + 1));
        int count = 0;
        for (int id : ids) {
            count += islands.get(id);
        }
        return 1 + count;
    }

    private int id(int[][] grid, int r, int c) {
        if (r < 0 || r >= grid.length || c < 0 || c >= grid[0].length) {
            return 0;
        }
        return grid[r][c];
    }
}
```
