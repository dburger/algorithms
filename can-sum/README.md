# Can Sum

Write a function `canSum(targetSum, numbers)` that takes in a `targetSum` and an
array of numbers as arguments. The function should return a boolean indicating
whether or not it is possible to generate the `targetSum` using numbers from the
array.

You may use an element of the array as many times as needed and you may assume
that all input numbers are nonnegative.

**Note:** This is similar to [How Sum](how-sum), [Best Sum](best-sum), and
[Combination Sum](combination-sum).

## Hints

1. Recursion with a reducing target. When it reduces to zero, you have a match.
   If it goes negative, you have reached a base case and can terminate that
   path.

## Solutions

### Recursive target reduction

This solution follows the hint above. Note that the performance here is bad and
you should feel bad. If n is the number of candidates and m is `target`, then
the time complexity is O(n^m). That is a fully filled out tree of invocations
would have a branching factor of n different candidates, and if one of those
were 1, the depth to reduce m to a base case would be m invocations. Meanwhile
the space complexity is O(m) to handle that recursive depth.

This solution is trivially improved with a memoizer below.

```java
class Solution {
    public boolean canSum(int[] candidates, int target) {
        if (target < 0) {
            return false;
        } else if (target == 0) {
            return true;
        }

        for (int c : candidates) {
            if (canSum(candidates, target - c)) {
                return true;
            }
        }

        return false;
    }
}
```

### Memoized recursive target reduction

This is the above solution with a memoizer thrown in to improve the time
complexity. The time complexity here is reduced to O(m * n) while the
space complexity remains O(m).

```java
class Solution {
    public boolean canSum(int[] candidates, int target) {
        Map<Integer, Boolean> memo = new HashMap<>();
        return canSum(candidates, target, memo);
    }

    private boolean canSum(int[] candidates, int target, Map<Integer, Boolean> memo) {
        if (memo.containsKey(target)) {
            return memo.get(target);
        } else if (target < 0) {
            return false;
        } else if (target == 0) {
            return true;
        }

        for (int c : candidates) {
            if (canSum(candidates, target - c, memo)) {
                memo.put(target, true);
                return true;
            }
        }

        memo.put(target, false);
        return false;
    }
}
```

### Dynamic programming table

Here we use the classic table approach to build up a solution from smaller
solutions.

```java
class Solution {
    public boolean canSum(int[] candidates, int target) {
        boolean[] table = new boolean[target + 1];
        // Can sum to zero by choosing no elements from candidates.
        table[0] = true;
        for (int i = 1; i <= target; i++) {
            for (int c : candidates) {
                if (i - c >= 0 && table[i - c]) {
                    table[i] = true;
                    break;
                }
            }
        }
        return table[target];
    }
}
```
