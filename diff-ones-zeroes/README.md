# Difference Between Ones and Zeroes in Row and Column

You are given a 0-indexed `m x n` binary matrix `grid`.

A 0-indexed `m x n` difference matrix `diff` is created with the following
procedure:

- Let the number of ones in the `ith` row be `onesRowi`.
- Let the number of ones in the `jth` column be `onesColi`.
- Let the number of zeroes in the `ith` row be `zeroesRowi`.
- Let the number of zeroes in the `jth` column be `zeroesColi`.
- `diff[i][j] = onesRowi + onesColj - zeroesRowi - zeroesColj`

Return the difference matrix `diff`.

## Hints

1. Nothing fancy here. Just count and compute.

## Solutions

### Count and compute

This solution just does a simple two pass approach. In the first pass the
counts are computed. Note that we only count the ones since the zeroes can
be computed as the row or column size minus the ones count.

The time complexity of this solution is O(m * n) where `m` is the number of
rows and `n` is the number of columns. This of course comes from the iterations
through `grid` to count and `diff` to produce the result. The space complexity
is O(max(m, n)). This comes from the arrays holding the row and column counts.

```java
class Solution {
    public int[][] onesMinusZeros(int[][] grid) {
        int m = grid.length;
        int n = grid[0].length;
        int[] rowOnes = new int[m];
        int[] colOnes = new int[n];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                int val = grid[i][j];
                rowOnes[i] += val;
                colOnes[j] += val;
            }
        }
        int[][] diff = new int[m][n];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                diff[i][j] = 2 * rowOnes[i] + 2 * colOnes[j] - m - n;
            }
        }
        return diff;
    }
}
```
