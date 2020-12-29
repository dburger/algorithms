# Combination Sum

Given an array of **distinct** integers `candidates` and a target integer
`target`, return a list of all **unique cobminations** of `candidates` where
the chosen numbers sum to `target`. You may return the combinations in any
order.

The **same** number may be chosen from `candidates` an **unlimited number of
times**. Two combinations are unique if the frequency of at least one of the
chosen numbers is different.

It is **gauranteed** that the number of unique combinations that sum up to
`target` is less than `150` combinations for the given input.

## Hints

1. A variation of combination generating code gets quite close to this solution.
   Think of a recursive solution that subtracts from target while proposing
   candidates.

## Solutions

### Recursive madness

This solution calls to mind some of the algorithms for generating permuations
and combinations. Note the common add to path, recurse, and remove from path.
Also note the usage of index to control which candidates are tried.

```java
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> acc = new ArrayList<>();
        List<Integer> path = new ArrayList<>();
        cs(path, candidates, target, 0, acc);
        return acc;
    }

    private void cs(List<Integer> path, int[] candidates, int target, int index,
            List<List<Integer>> acc) {
        if (target < 0) {
            return;
        } else if (target == 0) {
            acc.add(new ArrayList<>(path));
            return;
        }
        // index here prevents the same set in a different order from
        // being produced.
        for (int i = index; i < candidates.length; i++) {
            int candidate = candidates[i];
            path.add(candidate);
            cs(path, candidates, target - candidate, i, acc);
            path.remove(path.size() - 1);
        }
    }
}
```
