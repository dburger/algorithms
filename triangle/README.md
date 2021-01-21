# Triangle

Given a `triangle` array, return the minimum path sum from top to bottom.

For each step, you may move to an adjacent number of the row below. More
formally, if you are on index `i` on the current row, you may move to either
index `i` or index `i + 1` on the next row.

## Hints

1. This is very similar to many other problems that ask you to minimize or
   maximize as you traverse through a matrix. The only difference here is that
   this is not a square matrix but instead a triangle shape.
1. Obvious memoization and dynamic programming approaches work here.

## Solutions

### Brute force

This is a brute force. This is shown so that the transition to a memoized
approach below is very obvious. It is a beautifully simple piece of code,
that has poor performance because of repeated computation of substrees in
the triangle.

```java
class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        return minimumTotal(triangle, 0, 0);
    }

    private int minimumTotal(List<List<Integer>> triangle, int depth, int pos) {
        if (depth == triangle.size() || pos == depth + 1) {
            return 0;
        }

        int left = minimumTotal(triangle, depth + 1, pos, memo);
        int right = minimumTotal(triangle, depth + 1, pos + 1, memo);

        return Math.min(left, right) + triangle.get(depth).get(pos);
    }
}
```

### Memoize that shiz

This is the above brute force approach with the obvious memoizer thrown in. Note
that a fair amount of boilerplate is thrown in here for the `TriPos` class which
operates as the key in the memoization map. This class demonstrates the proper
implementation of the `equals` and `hashCode` methods.

```java
class Solution {
    private class TriPos {
        int depth;
        int pos;
        TriPos(int depth, int pos) {
            this.depth = depth;
            this.pos = pos;
        }

        @Override
        public boolean equals(Object o) {
            TriPos other = (TriPos) o;
            return other.depth == this.depth && other.pos == this.pos;
        }

        @Override
        public int hashCode() {
            int result = Integer.hashCode(this.depth);
            result = 31 * result + Integer.hashCode(this.pos);
            return result;
        }
    }

    public int minimumTotal(List<List<Integer>> triangle) {
        Map<TriPos, Integer> memo = new HashMap<>();
        return minimumTotal(triangle, 0, 0, memo);
    }

    private int minimumTotal(List<List<Integer>> triangle, int depth, int pos, Map<TriPos, Integer> memo) {
        if (depth == triangle.size() || pos == depth + 1) {
            return 0;
        }
        TriPos tp = new TriPos(depth, pos);
        if (memo.containsKey(tp)) {
            return memo.get(tp);
        }

        int left = minimumTotal(triangle, depth + 1, pos, memo);
        int right = minimumTotal(triangle, depth + 1, pos + 1, memo);

        int result = Math.min(left, right) + triangle.get(depth).get(pos);
        memo.put(tp, result);
        return result;
    }
}
```

### Dynamic programming

This solution uses a dynamic programming table approach to solve the problem.
An interesting aspect of this solution is that we create a full matrix but only
use a triangular portion of it.

It is really easy to mess up the indices in this problem. Be careful!

```java
class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        int rows = triangle.size();
        int cols = triangle.get(rows - 1).size();
        int[][] dp = new int[rows][cols];
        dp[0][0] = triangle.get(0).get(0);
        for (int row = 1; row < rows; row++) {
            for (int col = 0; col <= row; col++) {
                // If in bounds use it, otherwise provide value that will not be chosen.
                int leftParent = col - 1 > -1 ? dp[row - 1][col - 1] : Integer.MAX_VALUE;
                int rightParent = col < row ? dp[row - 1][col] : Integer.MAX_VALUE;
                dp[row][col] = Math.min(leftParent, rightParent) + triangle.get(row).get(col);
            }
        }

        // Finally locate the minimum in the last row.
        int[] dpr = dp[rows - 1];
        int min = dpr[0];
        for (int i = 1; i < rows; i++) {
            int val = dpr[i];
            if (val < min) {
                min = val;
            }
        }
        return min;
    }
}
```

### Dynamic programming, in place

Finally I present a solution that uses a dynamic programming approach and solves
the problem in place. Note that by solving in place the original input is
"destroyed."

```java
class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        int rows = triangle.size();
        for (int row = 1; row < rows; row++) {
            List<Integer> prior = triangle.get(row - 1);
            List<Integer> trow = triangle.get(row);
            for (int col = 0; col < trow.size(); col++) {
                int leftParent = col - 1 > -1 ? prior.get(col - 1) : Integer.MAX_VALUE;
                int rightParent = col < prior.size() ? prior.get(col) : Integer.MAX_VALUE;
                trow.set(col, Math.min(leftParent, rightParent) + trow.get(col));
            }
        }

        List<Integer> trow = triangle.get(rows - 1);
        int min = trow.get(0);
        for (int col = 1; col < trow.size(); col++) {
            int val = trow.get(col);
            if (val < min) {
                min = val;
            }
        }
        return min;
    }
}
```
