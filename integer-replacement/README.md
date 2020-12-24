# Integer Replacement

Given a positive integer `n`, you can apply one of the following operations:

1. If `n` is even, replace `n` with `n / 2`.
1. If `n` is odd, replace `n` with either `n + 1` or `n - 1`.

Return the minimum number operations needed for `n` to become `1`.

## Hints

1. Recursion, recursion, recursion. One of the recursive options needs to try
   `n + 1` and `n - 1` and go with the best one.
1. A memoizer will speed this up significantly.

## Solutions

### Memoized recursion

Recursion with memoization as in the hints. Note the edge case problem
avoidance by seeding the memoizer with `Integer.MAX_VALUE`. If this were not
done, an attempt at trying `Integer.MAX_VALUE + 1` would be made which would
cause a stack overflow. You could alternatively handle this edge case with
an explict check in the odd case branch.

```java
class Solution {
    public int integerReplacement(int n) {
        Map<Integer, Integer> memo = new HashMap<>();
        memo.put(1, 0);
        // Seeding this value prevents a run on Integer.MAX_VALUE + 1;
        memo.put(Integer.MAX_VALUE, 32);
        return ir(n, memo);
    }

    private int ir(int n, Map<Integer, Integer> memo) {
        if (memo.containsKey(n)) {
            return memo.get(n);
        } else if (n % 2 == 0) {
            int result = 1 + ir(n / 2, memo);
            memo.put(n, result);
            return result;
        } else {
            int result = 1 + Math.min(ir(n + 1, memo), ir(n - 1, memo));
            memo.put(n, result);
            return result;
        }
    }
}
```
