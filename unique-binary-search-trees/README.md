# Unique Binary Search Trees

Given an integer `n`, return the number of structurally unique binary
trees which has exactly `n` nodes.

## Hints

1. Think in terms of a tree with `n` nodes being constructed of the root
   node and trees with `0` to `n - 1` nodes in the left and right subtrees.
1. This is a great problem as it succumbs quite easily to the two common
   dynamic programming approaches.
1. Do the math. Knuth would know.

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

### Memoized with an array

Here is the same solution as the above with an array used for the memo instead
of a map.

```java
class Solution {
    public int numTrees(int n) {
        int[] memo = new int[n + 1];
        for (int i = 0; i < memo.length; i++) {
            memo[i] = -1;
        }
        memo[0] = 0;
        memo[1] = 1;
        return numTrees(1, n, memo);
    }

    private int numTrees(int start, int end, int[] memo) {
        if (start > end) {
            return 0;
        }
        int size = end - start + 1;
        int mval = memo[size];
        if (mval != -1) {
            return mval;
        }
        int count = 0;
        for (int i = start; i <= end; i++) {
            int left = numTrees(start, i - 1, memo);
            int right = numTrees(i + 1, end, memo);
            if (left == 0 || right == 0) {
                count += left + right;
            } else {
                count += left * right;
            }
        }
        memo[size] = count;
        return count;
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

### Doing the math, catalan numbers

Turns out reading Knuth would help on these problems. There is a recurrence relation that defines this
sequence. Unlike the "dynamic programming" approach above, this one features a one shot run through the
dp table. This is the recurrence relation from the
[Catalan Numbers wikipedia page](https://en.wikipedia.org/wiki/Catalan_number).

It would be difficult to memorize this or derive this during an interview. You might, however,
remember that this exists.

```java
class Solution {
    public int numTrees(int n) {
        int[] dp = new int[n + 1];
        dp[0] = 1;
        for (int i = 1; i < dp.length; i++) {
            // Simplified
            // dp[i] = (int) (dp[i - 1] * ((2 * (2 * (i - 1) + 1)) / (double) (i -1 + 2)));
            dp[i] = (int) (dp[i - 1] * ((4 * i - 2) / (double) (i + 1)));
        }
        return dp[n];
    }
}
```

### Doing the math, closed form formula

There is a closed form formula for this! Also found in Knuth and in the
[Catalan Numbers wikipedia page](https://en.wikipedia.org/wiki/Catalan_number).

```java
class Solution {
    public int numTrees(int n) {
        BigInteger numerator = combs(2 * n, n);
        return numerator.divide(BigInteger.valueOf(n + 1)).intValue();
    }

    private BigInteger combs(int n, int k) {
        BigInteger numerator = fact(n);
        BigInteger denominator = fact(k).multiply(fact(n - k));
        return numerator.divide(denominator);
    }

    private BigInteger fact(int n) {
        BigInteger result = BigInteger.ONE;
        while (n > 1) {
            result = result.multiply(BigInteger.valueOf(n--));
        }
        return result;
    }
}
```
