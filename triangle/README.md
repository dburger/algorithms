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
