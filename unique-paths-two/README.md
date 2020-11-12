# Unique Paths II

A robot is located at the top-left corner of an `m x n` grid.

The robot can only move either down or right at any point in time. The robot
is trying to reach the bottom-right corner of the grid.

Now consider if some obstacles are added to the grids. How many unique paths
would there be?

An obstacle and space is marked `1` and `0` respectively in the grid.

## Hints

1. This is just the classic dynamic programming approach for unique paths
   with the obstacle consideration thrown in. The algorithm is quite similar.

## Solutions

### Classic dynamic programming

```java
class Solution {
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        // Make a dynamic programming grid same size as input grid.
        int m = obstacleGrid.length;
        int n = obstacleGrid[0].length;
        int[][] paths = new int[m][n];
        // Mark the first cell as reachable or not reachable, depending on whether
        // it has an obstacle.
        paths[0][0] = obstacleGrid[0][0] == 1 ? 0 : 1;
        // Now mark the first column, reachable as long as does not contain obstacle
        // and above cell is reachable.
        for (int i = 1; i < m; i++) {
            paths[i][0] = obstacleGrid[i][0] == 1 || paths[i - 1][0] == 0 ? 0 : 1;
        }
        // Now mark the first row, reachable as long as does not contain obstacle
        // and left cell is reachable.
        for (int j = 1; j < n; j++) {
            paths[0][j] = obstacleGrid[0][j] == 1 || paths[0][j - 1] == 0 ? 0 : 1;
        }
        // Finally fill in the body, if obstacle 0 paths, otherwise, paths is sum of
        // paths to cell above and paths to cell to left.
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                paths[i][j] = obstacleGrid[i][j] == 1 ? 0: paths[i - 1][j] + paths[i][j - 1];
            }
        }
        return paths[m - 1][n - 1];
    }
}
```
