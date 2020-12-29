# How Sum

Write a function `howSum(targetSum, numbers)` that takes in a `targetSum` and an
array of numbers as arguments. The function should return any combination of
elements from numbers that sums up exactly to the `targetSum`. If there is no
such combination, the function should return null.

You may use an element of the array as many times as needed and you may assume
that all input numbers are nonnegative.

**Note:** This is similar to [Can Sum](can-sum), [Best Sum](best-sum), and
[Combination Sum](combination-sum).

## Hints

1. Recursion with a reducing target. When it reduces to zero, you have a match.
   If it goes negative, you have reached a base case and can terminate that
   path.

## Solutions

### Recursive target reduction

This solution follows the hint above.

```java
class Solution {
    private static List<Integer> howSum(int[] candidates, int target) {
        if (target == 0) {
            return new ArrayList<>();
        } else if (target < 0) {
            return null;
        }

        for (int c : candidates) {
            List<Integer> result = howSum(candidates, target - c);
            if (result != null) {
                result.add(c);
                return result;
            }
        }

        return null;
    }
}
```

### Memoized recursive target reduction

This is the above solution with a memoizer thrown in to improve the time
complexity.

```java
class Solution {
    public List<Integer> howSum(int[] candidates, int target) {
        return howSum(candidates, target, new HashMap<>());
    }

    private List<Integer> howSum(int[] candidates, int target, Map<Integer, List<Integer>> memo) {
        if (memo.containsKey(target)) {
            return memo.get(target);
        } else if (target == 0) {
            return new ArrayList<>();
        } else if (target < 0) {
            return null;
        }

        for (int c : candidates) {
            List<Integer> result = howSum(candidates, target - c, memo);
            if (result != null) {
                result.add(c);
                memo.put(target, result);
                return result;
            }
        }

        memo.put(target, null);
        return null;
    }
}
```
