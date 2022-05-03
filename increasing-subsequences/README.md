# Increasing Subsequences

Given an integer array `nums`, return all the different possible increasing
subsequences of the given array with **at least two elements**. You may return
the answer in **any order**.

The given array may contain duplicates, and two equal integers should also be
considered a special case of increasing sequence.

## Hints

1. Classic backtracking - remember to reject duplicates.

## Solutions

### Get back, get back, get backtracking

This solution uses a classic backtracking approach. It works by building the
subsequences while iterating across the input. The backtracking occurs when
a value qualifies as "increasing", is added, recursed, and then removed. The
rejection of duplicate entries is handled by accumulating into a `HashSet`.

For the time complexity we can think in terms of the number of solutions
generated. In the case where the array increases from left to right, any
selection of the array items results in a valid subsequence. The total number
of such subsequences is `2^n`. (Minus `n` and `1` since we only accept
subsequences of length `2` or greater.). Thus the time complexity of this
solution is O(2^n). Likewise, the space complexity to hold the solution is
O(2^n).

```java
class Solution {
    public List<List<Integer>> findSubsequences(int[] nums) {
        Set<List<Integer>> acc = new HashSet<>();
        fs(nums, 0, new ArrayList<>(), acc);
        return new ArrayList<>(acc);
    }

    private void fs(int[] nums, int pos, List<Integer> subs, Set<List<Integer>> acc) {
        if (subs.size() > 1) {
            acc.add(new ArrayList<>(subs));
        }

        for (int i = pos; i < nums.length; i++) {
            int val = nums[i];
            if (subs.isEmpty() || val >= subs.get(subs.size() - 1)) {
                subs.add(val);
                fs(nums, i + 1, subs, acc);
                subs.remove(subs.size() - 1);
            }
        }
    }
}
```
