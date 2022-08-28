# Rotate Image

You are given a `n x n` 2D `matrix` representing an image, rotate the image by
**90** degrees (clockwise).

You have to rotate the image in-place, which means you have to modify the input
2D matrix directly. **DO NOT** allocate another 2D matrix and do the rotation.

## Hints

1. Think four at a time.

## Solutions

### Four corners

This solution works by recording one of the "corners" in a temp value and then
rotating everything around. We use a nested loop with indices `i` and `j`. The
corners can then be rotated in the expected way. The tricky part is figuring
out - "when do I use `i` and when do I use `j` in the indices." Well, you need
to think of which index is moving faster for the corner in question. The inner
index should be used with the coordinate of the corner that is moving faster.

The time complexity of this solution is O(n^2) where `n` is the size of the
matrix. The space complexity of this solution is O(1).

```java
class Solution {
    public void rotate(int[][] matrix) {
        int n = matrix.length;
        for (int i = 0; i < (n + 1) / 2; i++) {
            for (int j = 0; j < n / 2; j++) {
                // Record bottom left.
                int temp = matrix[n - 1 - j][i];
                // Bottom left = bottom right.
                matrix[n - 1 - j][i] = matrix[n - 1 - i][n - 1 - j];
                // Bottom right = top right.
                matrix[n - 1 - i][n - 1 - j] = matrix[j][n - 1 - i];
                // Top right = top left.
                matrix[j][n - 1 - i] = matrix[i][j];
                // Top left = bottom left;
                matrix[i][j] = temp;
            }
        }
    }
}
```
