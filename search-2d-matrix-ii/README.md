# Search a 2D Matrix II

Write an efficient algorithm that searches for a `target` value in an `m x n`
integer `matrix`. The `matrix` has the following properties:

* Integers in each row are sorted in ascending order from left to right
* Integers in each column are sorted in ascending order from top to bottom.

## Hints

1. You want to think in terms of reducing the search space - much like a binary
   search does. What are some of the ways you can reduce the search space given
   this matrix layout?

## Solutions

### Rectangular chunks

This solution uses recursion as it reduces the search space. Here we look at the
`val` in `(midR, midC)`. If `val` is greater than `target`, then think
about where the solution has to lie. Since all the numbers in the same row to the
left, and in the same column to the top, including `(midR, midC)` itself are less
than target, that defines a rectangle of search space that can be eliminated.
Pictorially, with `v` for `val`, and `e` for eliminated, and `c` for candidate,
this equates to:

```text
e e e c c c
e e e c c c
e e v c c c
c c c c c c
c c c c c c
```

So the space left to search is that occupied by the remaining candidate cells `c`.
This can be achieved by splitting it up into two separate rectangles. The code
follows.

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int maxR = matrix.length - 1;
        int maxC = matrix[0].length - 1;
        return sm(matrix, 0, maxR, 0, maxC, target);
    }

    private boolean sm(int[][] matrix, int minR, int maxR, int minC, int maxC,
            int target) {
        if (minR > maxR || minC > maxC) {
            return false;
        }

        int midR = minR + (maxR - minR) / 2;
        int midC = minC + (maxC - minC) / 2;

        int val = matrix[midR][midC];
        if (target > val) {
            return sm(matrix, midR + 1, maxR, minC, midC, target) ||
                   sm(matrix, minR, maxR, midC + 1, maxC, target);
        } else if (target < val) {
            return sm(matrix, minR, maxR, minC, midC - 1, target) ||
                   sm(matrix, minR, midR - 1, midC, maxC, target);
        } else {
            return true;
        }
    }
}
```

### Proper starting points

The above solution works, and isn't overly complex, but you do have to code
the two sub-rectangle recursive invocations carefully with regards to the
indices. There is actually a way that is quite a bit simpler. If you start
in either the bottom left or top right corners, think about what the
`target` versus `val` comparison means. Let's consider the bottom left corner
case. In this case we start at `(row, col) == (matrix.length - 1, 0)` If `target`
is greater than `val`, then we know that everything in that same column can be
eliminated. We do this by recursing on `(row, col + 1)`. Similarly, if `target`
is less than `val`, then we know that everything in that same row can be
eliminated. We do this by recursing on `(row - 1, col)`. Thus on every iteration
we remove either a row or a column until we find the target.

This makes the time complexity easy to analyze as it is O(m + n) where m is the
number of rows and n is the number of columns. Here the space complexity is also
O(m + n) because of the recursive approach. If this had been done with iteration
then the space complexity would be O(1).

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int row = matrix.length - 1;
        int col = 0;
        return sm(matrix, row, col, target);
    }

    private boolean sm(int[][] matrix, int row, int col, int target) {
        if (row < 0 || col > matrix[0].length - 1) {
            return false;
        }

        int val = matrix[row][col];
        if (target > val) {
            return sm(matrix, row, col + 1, target);
        } else if (target < val) {
            return sm(matrix, row - 1, col, target);
        } else {
            return true;
        }
    }
}
```
