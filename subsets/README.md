# Subsets

Given an integer array `nums`, return all possible subsets (the power set).

The solution must not contain duplicate subsets.

## Hints

1. The bit approach makes this rather straightforward.

## Solutions

### Matching bits

Here we just walk from `0` to `2^n` to represent all bit arrangements and
include / exclude members based on the bits.

```java
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        int n = nums.length;
        int max = (int) Math.pow(2, n);
        List<List<Integer>> subsets = new ArrayList<>();
        for (int i = 0; i < max; i++) {
            List<Integer> subset = new ArrayList<>();
            for (int j = 0; j < n; j++) {
                if (bitset(i, j)) {
                    subset.add(nums[j]);
                }
            }
            subsets.add(subset);
        }
        return subsets;
    }

    private boolean bitset(int num, int bit) {
        return (num & (1 << bit)) > 0;
    }
}
```

## Cascading

Leeter calls this "cascading." This approach seeds things with an empty
set. Then the elements of `nums` are iterated over. In each iteration
a list of new subsets is generated that is the existing subsets plus
the new element. This new subset is then adding to existing subsets ready
for the next round.

```java
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> subsets = new ArrayList<>();
        // The empty set is always a member of the power set.
        subsets.add(new ArrayList<>());

        // For each member, add a new subset for each existing subset with this
        // additional member.
        for (int num : nums) {
            List<List<Integer>> newSubsets = new ArrayList<>();
            for (List<Integer> existing : subsets) {
                List<Integer> newSubset = new ArrayList<>(existing);
                newSubset.add(num);
                newSubsets.add(newSubset);
            }
            subsets.addAll(newSubsets);
        }

        return subsets;
    }
}
```
