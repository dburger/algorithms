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
    public List<Integer> howSum(int[] candidates, int target) {
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

### Dynamic programming table

Here we use the classic dynamic programming approach of building up a table.
The 0th entry is one empty list since selecting no elements of the candidates
sums up to 0. From there a look back is performed into the array at index
`i - candidate`. For each solution found there, an additional solution can
be had with a copy of that list plus the candidate. In this case we only
need to identify one solution so we break after adding any result.

```java
class Solution {
    public List<Integer> howSum(int[] candidates, int target) {
        List<List<Integer>> table = new ArrayList<>(target + 1);
        // To sum to 0, selecting none of the candidates works.
        List<Integer> empty = new ArrayList<>();
        table.add(0, empty);
        for (int i = 1; i <= target; i++) {
            table.add(i, null);
            for (int c : candidates) {
                if (i - c >= 0) {
                    List<Integer> l = table.get(i - c);
                    if (l != null) {
                        List<Integer> copy = new ArrayList<>(l);
                        copy.add(c);
                        table.add(i, copy);
                        // We only are looking for any one solution,
                        // so we'll skip the rest of the candidates
                        // now that we have found one.
                        break;
                    }
                }
            }
        }
        return table.get(target);
    }
}
```
