# Search a 2d Matrix

Write an efficient algorithm that searches for a value in an `m x n` matrix.
This matrix has the following properties:

* Integer in earch row are sorted from left to right.
* The first integer of each row is greater than the last integer of the previous
  row.

## Hints

1. The properties of the matrix make it a fully sorted array if flattened
   out. You can use this to do a binary search on it.

## Solutions

### Binary Search

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int rows = matrix.length;
        if (rows == 0) {
          return false;
        }
        int cols = matrix[0].length;
        int cells = rows * cols;
        int lo = 0;
        int hi = cells - 1;
        while (lo <= hi) {
            int mid = lo + (hi - lo) / 2;
            int row = mid / cols;
            int col = mid % cols;
            int val = matrix[row][col];
            if (val == target) {
              return true;
            } else if (val < target) {
              lo = mid + 1;
            } else {
              hi = mid - 1;
            }
        }
        return false;
    }
}class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int rows = matrix.length;
        if (rows == 0) {
          return false;
        }
        int cols = matrix[0].length;
        int cells = rows * cols;
        int lo = 0;
        int hi = cells - 1;
        while (lo <= hi) {
            int mid = lo + (hi - lo) / 2;
            int row = mid / cols;
            int col = mid % cols;
            int val = matrix[row][col];
            if (val == target) {
              return true;
            } else if (val < target) {
              lo = mid + 1;
            } else {
              hi = mid - 1;
            }
        }
        return false;
    }
}
```
