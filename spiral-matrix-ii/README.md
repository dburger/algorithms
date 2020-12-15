# Spiral Matrix II

Given a positive integer `n`, generate an `n x n matrix` filled with elements
from `1` to `n^2` in spiral order.

## Hints

1. Much like [Spiral Matrix](spiral-matrix), indices for:
   * The lowest row yet to work on.
   * The highest row yet to work on.
   * The lowest column yet to work on.
   * The highest column yet to work on.
   Should serve you well.

## Solutions

### All those indices

This code just follows the hint. The running time of this algorithm is obviously
O(n^2) as each cell must be filled in.

```java
class Solution {
    public int[][] generateMatrix(int n) {
        int rLo = 0;
        int rHi = n - 1;
        int cLo = 0;
        int cHi = n - 1;
        int val = 1;
        int[][] result = new int[n][n];
        while (true) {
            // Top row.
            for (int c = cLo; c <= cHi; c++) {
                result[rLo][c] = val++;
            }
            if (++rLo > rHi) break;
            // Right col.
            for (int r = rLo; r <= rHi; r++) {
                result[r][cHi] = val++;
            }
            if (cLo > --cHi) break;
            // Bottom row.
            for (int c = cHi; c >= cLo; c--) {
                result[rHi][c] = val++;
            }
            if (rLo > --rHi) break;
            // Left col.
            for (int r = rHi; r >= rLo; r--) {
                result[r][cLo] = val++;
            }
            if (++cLo > cHi) break;
        }
        return result;
    }
}
```
