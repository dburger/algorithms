# Pascal's Triangle

Given a non-negative integer `numRows`, generate the first `numRows` rows of
Pascal's triangle.

In Pascal's triangle, each number is the sum of the two numbers directly above
it.


## Hints

1. This is pretty mechanical. If they asked you to generate just one row you
   could use the formulate for combinations. Here since they are asking you for
   everything it is just as fast to directly build the triangle.

## Solutions

### Build er up

This solution mechanically follows the hint, building the triangle up from the
prior rows.

This solution has time complexity O(n^2) where `n == numRows`, since there each
entry must be calculated and each row may have up to `n` items. Likewise the
space complexity is O(n^2) to hold this result.

```java
class Solution {
    public List<List<Integer>> generate(int numRows) {
        List<List<Integer>> result = new ArrayList<>();
        if (numRows == 0) {
            return result;
        }
        List<Integer> newRow = new ArrayList<>();
        newRow.add(1);
        result.add(newRow);
        for (int row = 1; row < numRows; row++) {
            List<Integer> priorRow = newRow;
            newRow = new ArrayList<>();
            result.add(newRow);
            for (int col = 0; col <= row; col++) {
                int left = col == 0 ? 0 : priorRow.get(col - 1);
                int right = col == row ? 0 : priorRow.get(col);
                newRow.add(left + right);
            }
        }
        return result;
    }
}
```
