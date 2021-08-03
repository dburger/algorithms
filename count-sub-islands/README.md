# Count Sub Islands

You are given two `m x n` binary matrics `grid1` and `grid2` containing only
`0`'s (representing water) and `1`'s (representing land). An **island** is a
group of `1`'s connected **4-directionally** (horizontal or vertical). Any
cells outside of the grid are considered water cells.

An island in `grid2` is considered a **sub-island** if there is an island in
`grid1` that contains **all** the cells that make up **this** island in
`grid2`.

Return the **number** of islands in `grid2` that are considered
**sub-islands**.

## Hints

1. These islands in matrices problems usually start with being able to
   identify / mark the different islands. Start there.

## Solutions

### Mark and eliminate

Here we follow what basically amounts to a three phase approach. These phases
are:

1. Mark the islands in `grid2`
1. Iterate through the cells of `grid1` and for cells that are land, "blank
   out" the corresponding cell of `grid2`.
1. Iterate through `grid2` to determine which islands have been completely
   blanked out. These are the sub-islands.

The details are many. Because the input uses `0` to mark water and `1` to mark
land we start our islanding marking in `grid2` with `2`. To "blank out" cells
of `grid2` in the second phase we use `-1`.

The time comlexity of this algorithm is O(m * n) where m is the number or rows
in a grid and n is the number of columns. This comes from the three iterations
through the grids. The space complexity for this algorithm is O(m x n) as an
upper bound on the number of cells left for each island in the checking phase.

```java
class Solution {
    public int countSubIslands(int[][] grid1, int[][] grid2) {
        // Mark the islands in grid2 starting with 2. The number of
        // islands found will be mark - 2.
        int mark = 2;
        for (int r = 0; r < grid2.length; r++) {
            for (int c = 0; c < grid2[0].length; c++) {
                if (mark(grid2, r, c, mark)) {
                    mark++;
                }
            }
        }

        // Now blank out each cell in grid2 where a corresponding grid1
        // cell has land.
        for (int r = 0; r < grid2.length; r++) {
            for (int c = 0; c < grid2[0].length; c++) {
                if (grid1[r][c] == 1) {
                    grid2[r][c] = -1;
                }
            }
        }

        // Now count how many land cells remain for each island marked
        // in grid2. Note that island 2 is at position 0, ..., etc.
        int[] islands = new int[mark - 2];
        for (int r = 0; r < grid2.length; r++) {
            for (int c = 0; c < grid2[0].length; c++) {
                int val = grid2[r][c];
                if (val > 0) {
                    islands[val - 2]++;
                }
            }
        }

        // Each island that no longer has land cells was a sub-island
        int count = 0;
        for (int i = 0; i < islands.length; i++) {
            if (islands[i] == 0) {
                count++;
            }
        }
        return count;
    }

    private boolean mark(int[][] grid, int r, int c, int mark) {
        if (grid[r][c] == 1) {
            expand(grid, r, c, mark);
            return true;
        } else {
            return false;
        }
    }

    private void expand(int[][] grid, int r, int c, int mark) {
        if (r < 0 || r >= grid.length || c < 0 || c >= grid[0].length) {
            return;
        }
        if (grid[r][c] != 1) {
            return;
        }
        grid[r][c] = mark;
        expand(grid, r + 1, c, mark);
        expand(grid, r - 1, c, mark);
        expand(grid, r, c + 1, mark);
        expand(grid, r, c - 1, mark);
    }
}
```

### Mark and count

Whoa that last solution was ok but that is a lot of code and 3, count em, 3
passes through the grids!

Here we improve things by doing a single pass. The keys are that we don't
bother marking the separate islands instead we expand land cells back to
water cells (`1` gets marked to `0`) as we work through the pass. Also in the
pass we check if `grid1` mirrors the current island with a land cell without
short circuiting out of the evaluation if it does not. This is because we we
fully mark `grid2`'s islands or the left over bits will appear as separate
islands in the remaining iteration.

These improvements don't improve the big O time complexity, it remains
O(m * n) as we still must do a pass through the grids. In practice, however,
we are slightly faster as the number of passes is reduced to 1. The space
complexity for this solution is improved to O(1), as we do all of our marking
directly into the input grids.

```java
class Solution {
    public int countSubIslands(int[][] grid1, int[][] grid2) {
        int count = 0;
        for (int r = 0; r < grid2.length; r++) {
            for (int c = 0; c < grid2[0].length; c++) {
                if (check(grid1, grid2, r, c)) {
                    count++;
                }
            }
        }
        return count;
    }

    private boolean check(int[][] grid1, int[][] grid2, int r, int c) {
        if (grid2[r][c] == 1) {
            return expand(grid1, grid2, r, c);
        } else {
            return false;
        }
    }

    private boolean expand(int[][] grid1, int[][] grid2, int r, int c) {
        if (r < 0 || r >= grid2.length || c < 0 || c >= grid2[0].length) {
            return true;
        }
        if (grid2[r][c] != 1) {
            return true;
        }
        grid2[r][c] = 0;
        boolean result = grid1[r][c] == 1;
        result &= expand(grid1, grid2, r + 1, c);
        result &= expand(grid1, grid2, r - 1, c);
        result &= expand(grid1, grid2, r, c + 1);
        result &= expand(grid1, grid2, r, c - 1);
        return result;
    }
}
```
