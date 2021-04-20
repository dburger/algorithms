# Unique Binary Search Trees

Given an integer `n`, return the number of structurally unique binary
trees which has exactly `n` nodes.

## Hints

1. Think in terms of a tree with `n` nodes being constructed of the root
   node and trees with `0` to `n - 1` nodes in the left and right subtrees.

## Solutions

### Recursive

First I give a recursive solution. I will show this unmemoized to make the
structure clear. This would fail the leeter time constraints. I augment this
with a dynamic programming approach in the next solution to fix the time
constraints.

```java
class Solution {
    public int numTrees(int n) {
        if (n == 0) {
            return 0;
        }
        if (n == 1) {
            return 1;
        }
        int num = 0;
        // Step through every combination of contributing nodes to the left
        // and right subtrees.
        for (int i = 0; i < n; i++) {
            int left = numTrees(i);
            int right = numTrees(n - 1 - i);
            if (left == 0 || right == 0) {
                // If either side has zero nodes, then the additional
                // tree structures is the adding of the other.
                num += left + right;
            } else {
                // If both sides has greater than zero nodes, the the total
                // added configurations is multiplication of left and right.
                num += left * right;
            }
        }
        return num;
    }
}
```

### Memoized recursive

The above is a great recursive solution, however, because it will recompute
so many counts it is dog slow. The is very much like the redundant computations
in naive recursive Fibonacci algorithms. This can be fixed with the dynamic
programming memoization approach.

```java
class Solution {
    public int numTrees(int n) {
        Map<Integer, Integer> memo = new HashMap<>();
        // Base cases.
        memo.put(0, 0);
        memo.put(1, 1);
        return numTrees(n, memo);
    }

    private int numTrees(int n, Map<Integer, Integer> memo) {
        if (memo.containsKey(n)) {
            return memo.get(n);
        }
        int num = 0;
        // Step through every combination of contributing nodes
        // to the left and right subtrees.
        for (int i = 0; i < n; i++) {
            int left = numTrees(i, memo);
            int right = numTrees(n - 1 - i, memo);
            if (left == 0 || right == 0) {
                // If either side has zero nodes, then the additional
                // tree structures is the adding of the other.
                num += left + right;
            } else {
                // If both sides has greater than zero nodes, the the total
                // added configurations is multiplication of left and right.
                num += left * right;
            }
        }
        memo.put(n, num);
        return num;
    }
}
```

### Classic dynamic programming

Alternatively, this problem can also be solved with the typical table
dynamic programming approach. Here the table is seeded with the base cases
and proceeds by filling out the table to position `n` and returning the
value at that entry.

```java
class Solution {
    public int numTrees(int n) {
        int[] dp = new int[n + 1];
        dp[0] = 0;
        dp[1] = 1;
        for (int i = 2; i <= n; i++) {
            for (int j = 0; j < i; j++) {
                int left = dp[j];
                int right = dp[i - 1 - j];
                if (left == 0 || right == 0) {
                    // If either side has zero nodes, then the additional
                    // tree structures is the adding of the other.
                    dp[i] += left + right;
                } else {
                    // If both sides has greater than zero nodes, the the total
                    // added configurations is multiplication of left and right.
                    dp[i] += left * right;
                }
            }
        }
        return dp[n];
    }
}
```
