# Permutations II

Given a collection of numbers, `nums`, that might contain duplicates, return
all possible *unique permutations* **in any order**.

## Hints

1. This is pretty much [Permutations](../permutations) with a twist of no
   duplicates. Those solutions may be good starting points.

## Solutions

### Once per position

This solution follows the hint above, tweaking a solution from the similar
permutations problem referenced above. But how to adjust? Well,
think about how those solutions worked that weren't constained to
*unique permutations*. They fill a position at a time, as they iterate through
a:

*   add digit
*   recurse
*   remove digit

loop. To prevent duplicates we just make sure that we don't fill a position
with different input digits that have the same value. We can do this by first
sorting the input and then skipping past duplicate inputs in the primary loop.
Source follows.

```java
class Solution {
    public List<List<Integer>> permuteUnique(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> acc = new ArrayList<>();
        int len = nums.length;
        permute(nums, new boolean[len], new ArrayList<>(), acc);
        return acc;
    }

    private static void permute(int[] nums, boolean[] used, List<Integer> consumed, List<List<Integer>> acc) {
        if (consumed.size() == nums.length) {
            acc.add(new ArrayList<>(consumed));
            return;
        }
        for (int i = 0; i < nums.length; i++) {
            if (used[i]) {
                continue;
            }
            consumed.add(nums[i]);
            used[i] = true;
            permute(nums, used, consumed, acc);
            consumed.remove(consumed.size() - 1);
            used[i] = false;
            // For position i, the value at nums[i] has been used there once.
            // Here we move past all elements of the same value (nums is
            // sorted). This prevents duplicate entries from being generated.
            while (i < nums.length - 1 && nums[i] == nums[i + 1]) {
                i++;
            }
        }
    }
}
```
