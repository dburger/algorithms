# Unique Paths

A robot is located at the top-left corner of an `m x n` grid.

The robot can only move either down or right at any point in time. The robot is
trying to reach the bottom-right corner of the grid.

How many possible unique paths are there?

## Hints

1. Another classic dynamic programming problem. Filling in a grid with the
   unique paths up to cell (i, j) will do the trick.

## Solutions

### Classic dynamic programming

```java
class Solution {
    public int uniquePaths(int m, int n) {
        // Dynamic programming grid.
        int dp[][] = new int[m][n];
        // The first column has one path to each cell.
        for (int i = 0; i < m; i++) {
            dp[i][0] = 1;
        }
        // Likewise for the first row, and we can skip (0, 0) as we
        // have already filled it in above.
        for (int j = 1; j < n; j++) {
            dp[0][j] = 1;
        }
        // Now filling in the body.
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                // Each cell has the number of paths to it that is the
                // number of paths from above, plus the number of paths
                // from the left.
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
            }
        }
        return dp[m - 1][n - 1];
    }
}
```
