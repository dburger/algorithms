# Set Matrix Zeroes

Given an `m x n` matrix, if an element is `0`, set its entire row and column to
`0`. Do it in-place.

Follow up:

* A straight forwared solution using O(mn) space is probably a bad idea.
* A simple improvement uses O(m + n) space, but still not best solution.
* Couuld you devise a constant space solution?

## Hints

1. A simple algorithm could iterate and keep a set of columns and rows
   that should be zeroed out on a second pass.
1. A constant space solution could use the matrix itself to mark which columns
   and rows should be zeroed out. In this approach, however, one must be careful
   in how one keeps track of what to zero out.

## Solutions

### Mark in the matrix

In this approach the matrix is marked in the zeroth column or row to indicate
what to zero out on a second pass. To not overspecify, the zeroth column and
rows themselves, are handled separately.

```java
class Solution {
    public void setZeroes(int[][] matrix) {
        // First we note what to do with the first column and row
        // and keep track in boolean variables.
        boolean firstCol = false;
        boolean firstRow = false;
        if (matrix[0][0] == 0) {
            firstCol = true;
            firstRow = true;
        } else {
            for (int i = 1; i < matrix.length; i++) {
                if (matrix[i][0] == 0) {
                    firstCol = true;
                    break;
                }
            }
            for (int j = 1; j < matrix[0].length; j++) {
                if (matrix[0][j] == 0) {
                    firstRow = true;
                    break;
                }
            }
        }
        // Now iterating the body, outside of the first column and
        // row, we mark with a 0 in fist column or row if we find
        // a zero.
        for (int i = 1; i < matrix.length; i++) {
            for (int j = 1; j < matrix[0].length; j++) {
                if (matrix[i][j] == 0) {
                    matrix[i][0] = 0;
                    matrix[0][j] = 0;
                }
            }
        }
        // Now iterating the body again, oustide of the first column
        // and row, we set to 0 if the zero column or row indicates
        // as such.
        for (int i = 1; i < matrix.length; i++) {
            for (int j = 1; j < matrix[0].length; j++) {
                if (matrix[i][0] == 0 || matrix[0][j] == 0) {
                    matrix[i][j] = 0;
                }
            }
        }
        // Finally we go back to the variables to see what to do with
        // the first column and row.
        if (firstCol == true) {
            for (int i = 0; i < matrix.length; i++) {
                matrix[i][0] = 0;
            }
        }
        if (firstRow == true) {
            for (int j = 0; j < matrix[0].length; j++) {
                matrix[0][j] = 0;
            }
        }
    }
}
```
