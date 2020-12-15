# Spiral Matrix

Given an `m x n` matrix, return all the elements of the matrix in spiral order.

## Hints

1. There isn't that much to this one. Just think in terms of keeping indices
   of where you are in terms of:
   * Lowest row yet to work on.
   * Highest row yet to work on.
   * Lowest column yet to work on.
   * Highest column yet to work on.
   Use a loop that does one round of the spiral at a time.

## Solutions

### Four indices

Implemented per the hint. The running time of this would simply be
O(rows * cols), as it retrieves each value and marshalls it to the
returned array.

```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        int rLo = 0;
        int rHi = matrix.length - 1;
        int cLo = 0;
        int cHi = matrix[0].length - 1;
        List<Integer> result = new ArrayList<>();
        int fill = 0;
        while (true) {
            // Top row.
            for (int c = cLo; c <= cHi; c++) {
                result.add(matrix[rLo][c]);
            }
            if (++rLo > rHi) break;
            // Right column.
            for (int r = rLo; r <= rHi; r++) {
                result.add(matrix[r][cHi]);
            }
            if (cLo > --cHi) break;
            // Bottom row.
            for (int c = cHi; c >= cLo; c--) {
                result.add(matrix[rHi][c]);
            }
            if (rLo > --rHi) break;
            // Left column.
            for (int r = rHi; r >= rLo; r--) {
                result.add(matrix[r][cLo]);
            }
            if (++cLo > cHi) break;
        }
        return result;
    }
}
```
