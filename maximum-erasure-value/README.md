# Maximum Erasure Value

You are given an array of positive integers `nums` and want to erase a
subarray containing **unique elements**. The **score** you get by erasing the
subarray is equal to the **sum** of its elements.

Return the **maximum score** you get by erasing **exactly one** subarray.

An array `b` is a subarray of `a` if it forms a contiguous subsequence of `a`,
that is, if it is equal to `a[l], a[l + 1], ..., a[r]` for some `(l, r)`.

## Hints

1. This problem is worded very strangely. This problem stated more simply is
   given the sum for the maximum contiguous subarray sum with no duplicate
   numbers.
1. This is a classic sliding window algorithm problem.

## Solutions

### Slide that window

Here we give the classic sliding window solution. Left and right indicators
`l` and `r` are set up along with a `seen` set to identify duplicates. We
move `r` on each loop. If `r` adds in a duplicate, we slide `l` to the right
until the duplicate has been removed. All the while we keep track of the
current sum and max.

The time and space complexity of this algorithm is O(n) where n is the length
of the input `nums`. The time complexity comes from moving `l` and `r` across
the input. The space complexity comes from the identification of duplicates in
the `seen` set.

```java
class Solution {
    public int maximumUniqueSubarray(int[] nums) {
        Set<Integer> seen = new HashSet<>();
        int max = Integer.MIN_VALUE;
        int l = 0;
        int r = 0;
        int sum = 0;
        while (r < nums.length) {
            int val = nums[r];
            while (seen.contains(val)) {
                int lVal = nums[l];
                seen.remove(lVal);
                sum -= lVal;
                l++;
            }
            seen.add(val);
            sum += val;
            if (sum > max) {
                max = sum;
            }
            r++;
        }
        return max;
    }
}
```

### Sliding with performance tweaks

Here we take the prior solution and add some performance tweaks to make it
faster. The things we tweak are:

*   If the current subarray doesn't contain the new `val`, we avoid the
    removal dance entirely.
*   If the current subarray does contain the new `val` we don't remove it and
    add it back in.
*   If the current subarray does contain the new `val` we don't check if we
    are setting the new max because the sum will not increase.
*   We only check for a new max when the sum has increased.

These tweaks do not improve the big O in time or space from the prior solution.
In practice, however, they do increase the speed enough to be noticed by
leeter's graders.

```java
class Solution {
    public int maximumUniqueSubarray(int[] nums) {
        Set<Integer> seen = new HashSet<>();
        int max = Integer.MIN_VALUE;
        int l = 0;
        int r = 0;
        int sum = 0;
        while (r < nums.length) {
            int val = nums[r];
            if (seen.contains(val)) {
                int lVal = nums[l++];
                while (lVal != val) {
                    seen.remove(lVal);
                    sum -= lVal;
                    lVal = nums[l++];
                }
            } else {
                seen.add(val);
                sum += val;
                if (sum > max) {
                    max = sum;
                }
            }
            r++;
        }
        return max;
    }
}
```
