# Best Sum

Write a function `bestSum(targetSum, numbers)` that takes in a `targetSum`
and an array of numbers as arguments. The function should return an array
containing the **shortest** combination of numbers that add up to exactly
the `targetSum`. If there is a tie for the shortest combination, you may
return any one of the shortest.

**Note:** This is similar to [Can Sum](can-sum), [How Sum](how-sum), and
[Combination Sum](combination-sum).

## Hints

## Solutions

### Recursive target reduction

Here we use recursion and compare the returned lengths for all possible
solutions. The shortest is returned back up the invocation tree.

```java
class Solution {
    public List<Integer> bestSum(int[] candidates, int target) {
        if (target < 0) {
            return null;
        } else if (target == 0) {
            return new ArrayList<>();
        }

        List<Integer> shortest = null;

        for (int c : candidates) {
            List<Integer> result = bestSum(candidates, target - c);
            if (result != null) {
                result.add(c);
                if (shortest == null || result.size() < shortest.size()) {
                    shortest = result;
                }
            }
        }

        return shortest;
    }
}
```

### Memoized recursive target reduction

Same as above, but memoized for speed.

```java
class Solution {
    public List<Integer> bestSum(int[] candidates, int target) {
        return bestSum(candidates, target, new HashMap<>());
    }

    private List<Integer> bestSum(int[] candidates, int target, Map<Integer, List<Integer>> memo) {
        if (memo.containsKey(target)) {
            return memo.get(target);
        } else if (target < 0) {
            return null;
        } else if (target == 0) {
            return new ArrayList<>();
        }

        List<Integer> shortest = null;

        for (int c : candidates) {
            List<Integer> result = bestSum(candidates, target - c, memo);
            if (result != null) {
                result.add(c);
                if (shortest == null || result.size() < shortest.size()) {
                    shortest = result;
                }
            }
        }

        memo.put(target, shortest);
        return shortest;
    }
}
```
