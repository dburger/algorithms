# Unique Paths

A robot is located at the top-left corner of an `m x n` grid.

The robot can only move either down or right at any point in time. The robot is
trying to reach the bottom-right corner of the grid.

How many possible unique paths are there?

## Hints

1. Dynamic programming approach one - can you think of the base cases with
   memoization?
1. Another classic dynamic programming problem approach, filling in a grid with
   the unique paths up to cell (i, j) will do the trick.

## Solutions

### Dynamic programming memoization

This solution works by reducing the matrix size and working back to base cases
with classic recursion. A memoizer is thrown in to prevent repeated computation.
Moving down is equivalent to reducing the matrix by one row. Moving to the right
is equivalent to reducing the matrix by one col. The base cases are when you are
reduced to one row or one column. In each case, there is only one path to the
end.

Here we got fancy (read proper) and added a `Matrix` type, with the required
`equals` and `hashCode` methods, to function as a proper `Map` key. We could
have gone way simpler and just encoded a string as `${row}-${col}` but this is
a bit nicer.

```java
class Solution {
    // This is set up for the memoizer key.
    private class Matrix {
        int rows;
        int cols;
        Matrix(int rows, int cols) {
            this.rows = rows;
            this.cols = cols;
        }

        @Override
        public boolean equals(Object o) {
            Matrix other = (Matrix) o;
            return other.rows == this.rows && other.cols == this.cols;
        }

        @Override
        public int hashCode() {
            int result = Integer.hashCode(this.rows);
            result = 31 * result + Integer.hashCode(this.cols);
            return result;
        }
    }

    public int uniquePaths(int m, int n) {
        Map<Matrix, Integer> memo = new HashMap<>();
        return up(m, n, memo);
    }

    private int up(int m, int n, Map<Matrix, Integer> memo) {
        Matrix key = new Matrix(m, n);
        if (memo.containsKey(key)) {
            return memo.get(key);
        }
        // Invalid matrix.
        if (m == 0 || n == 0) return 0;
        // One row or one column, only one path to the end cell.
        if (m == 1 || n == 1) return 1;
        int result = up(m - 1, n, memo) + up(m, n - 1, memo);
        memo.put(key, result);
        return result;
    }
}
```

### Classic dynamic programming table

This approach uses a classic dynamic programming table approach. The entry
in `(r, c)` gives the unique paths to that zero based row and col position.
It is seeded by filling row 0 and col 0 with all ones. Then for the remaining
`(r, c)` each entry can be derived as `(r - 1, c) + (r, c - 1)`. That is, the
addition of the ways coming from moving down and moving to the right.

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
