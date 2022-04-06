# Check if Every Row and Column Contains All Numbers

An `n x n` matrix is valid if every row and every column contains all the
integers from `1` to `n` (inclusive).

Given an `n x n` integer matrix `matrix`, return `true` if the matrix is valid.
Otherwise return `false`.

## Hints

1. The sum will be the old familar `n * (n + 1) / 2` but is that enough?

## Solutions

### Double fantasy matrix

No it certainly isn't enough. Consider a matrix filled with `[1, 3]`. Here we
have `n * (n + 1) / 2 == 6`, however `2 + 2 + 2 == 6`. So much for a quick and
dirty summation of rows and columns.

How about using a "marker matrix." That is we use a matrix and mark in
`(i, j)` when row `i` has seen value `j`. Then we can proceed to check that
we see each value once and only once. This is close but has one problem. One
matrix does not suffice. From the previous example, marking column `j` to have
seen value `i` uses the same marking cell. Thus we use two marker matrices,
one for the rows and one for the columns. To keep things simple we'll mark
by keeping counts. The check will then be that each marker cell ends up with
a value of `1`.

The time complexity of this solution is O(n^2) as it requires two `n x n`
traversals. The first one is used to mark, the second one is used to check.
Likewise, the space complexity is also O(n^2) as necessary for sizing the
marker matrices. Note that we could check counts and reject immediately if
they go to two. This would cause some cases to exit early but wouldn't reduce
the time complexity from the O(n^2).

```java
class Solution {
    public boolean checkValid(int[][] matrix) {
        int n = matrix.length;
        int[][] ccounts = new int[n][n];
        int[][] rcounts = new int[n][n];
        for (int row = 0; row < n; row++) {
            for (int col = 0; col < n; col++) {
                // -1 to translate 1 based to 0 based
                int val = matrix[row][col] - 1;
                ccounts[val][col]++;
                rcounts[row][val]++;
            }
        }
        for (int row = 0; row < n; row++) {
            for (int col = 0; col < n; col++) {
                if (ccounts[row][col] != 1 || rcounts[row][col] != 1) {
                    return false;
                }
            }
        }
        return true;
    }
}
```

### Reduce the space

The prior solution is pretty simple and works well enough but we can do better
on the space complexity. This is done by working on a single row or column at
a time and thus we only allocate O(n) space. The time complexity remains as
before.

```
class Solution {
    public boolean checkValid(int[][] matrix) {
        int n = matrix.length;
        for (int row = 0; row < n; row++) {
            int[] check = new int[n];
            for (int col = 0; col < n; col++) {
                int val = matrix[row][col] - 1;
                check[val]++;
            }
            for (int i = 0; i < n; i++) {
                int val = check[i];
                if (val != 1) {
                    return false;
                }
            }
        }

        for (int col = 0; col < n; col++) {
            int[] check = new int[n];
            for (int row = 0; row < n; row++) {
                int val = matrix[row][col] - 1;
                check[val]++;
            }
            for (int i = 0; i < n; i++) {
                int val = check[i];
                if (val != 1) {
                    return false;
                }
            }
        }

        return true;
    }
}
```
