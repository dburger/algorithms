# Combination Sum II

Given a collection of candidate numbers (`candidates`) and a target number
(`target`), find all unique combinations in `candidates` where the candidate
numbers sum to `target`.

Each number in `candidates` may only be used **once** in the combination. The
solution set must not contain duplicate numbers.

## Hints

1. Much like [Combination Sum](../combination-sum) but disallows using the same
   candidate more than once and disallows duplicate numbers. How can you skip
   using the same value when candidates has duplicates?

## Solutions

### Sort and skip

This solution works by first sorting the `candidate` input array. This allows
an algorithm similar to [Combination Sum](../combination-sum) to work by
skipping candidates with the same value. Recursive calls include the int `pos`
which indicates where to start in the array for producing the next potential
solution. This prevents retrying duplicate combinations.

```java
class Solution {
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        Arrays.sort(candidates);
        List<List<Integer>> acc = new ArrayList<>();
        cs(candidates, 0, target, new ArrayList<>(), acc);
        return acc;
    }

    private void cs(int[] candidates, int pos, int target, List<Integer> path, List<List<Integer>> acc) {
        if (target <= 0) {
            if (target == 0) {
                acc.add(new ArrayList<>(path));
            }
            return;
        }
        for (int i = pos; i < candidates.length; i++) {
            int val = candidates[i];
            path.add(val);
            cs(candidates, i + 1, target - val, path, acc);
            path.remove(path.size() - 1);
            // Prevent using duplicate combinations (candidates is sorted).
            while (i < candidates.length - 1 && candidates[i + 1] == val) {
                i++;
            }
        }
    }
}
```
