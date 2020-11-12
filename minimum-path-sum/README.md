# Minimum Path Sum

Given an `m x n grid` filled with non-negative numbers, find a path from top
left to bottom right, which minimizes the sum of all numbers along its path.

**Note**: You can only move either down or right at any point in time.

## Hints

1. This is a classic dynamic programming problem. You can build a solution by
   filling in a grid with the best solution to that cell.

## Solutions

### Classic Dynamic Programming

The classic solution merely fills in the dynamic programming memo grid. This
solution is O(m x n) to fill out the grid and return the final cell.

```java
class Solution {
    public int minPathSum(int[][] grid) {
        // Make dynamic programming memo grid called score.
        int m = grid.length;
        int n = grid[0].length;
        int score[][] = new int[m][n];
        // The first cell's score comes from just one cell.
        score[0][0] = grid[0][0];
        // First column score only depends on above input.
        for (int i = 1; i < m; i++) {
            score[i][0] = score[i - 1][0] + grid[i][0];
        }
        // First row score only depends on left input.
        for (int j = 1; j < n; j++) {
            score[0][j] = score[0][j - 1] + grid[0][j];
        }
        // Now fill in the body taking the minimum of coming from above
        // or coming from the right.
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                score[i][j] = Math.min(score[i - 1][j], score[i][j - 1]) + grid[i][j];
            }
        }
        // Finally the score is now in the final cell.
        return score[m - 1][n - 1];
    }
}
```
