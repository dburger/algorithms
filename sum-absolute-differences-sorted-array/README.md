# Sum of Absolute Differences in a Sorted Array

You are given an integer array `nums` sorted in **non-decreasing** order.

Build and return an integer array `result` with the same length of `nums` such
that `result[i]` is equal to the summation of absolute differences between
`nums[i]` and all the other elements in the array.

In other words, `result[i]` is equal to `sum(abs(nums[i] - nums[j]))` where
`0 <= j <= nums.length` and `j != i`.

## Hints

1. Look at a pattern of the result entries.
1. OK, looking deeper, say the entry `result[i]`. The summation that forms that
   has a term `abs(nums[i] - nums[i])` which is of course `0`. Now look at the
   entries before that term, and the entries after that term. Can a pattern
   be made to sum those entries?

## Solutions

### Prefix to suffix

This solution follows the hints above. If you look at the result entry for `i`
you will notice that all the terms before `abs(nums[i] - nums[i])` will be
`nums[i]` minus a smaller value (because the array is sorted). All of the terms
after `abs(nums[i] - nums[i])` will be `nums[i]` minus a larger value.
Furthermore, there are `i` before terms, and `len(nums) - i - 1` after terms.
This gives us a way to construct the summations. We do this by first building
`prefix` and `suffix` arrays that give the sums of the arrays before and after
any position `i`. Then we use these sums in the formulas respecting the
relationship described above.

The time complexity of this solution is O(n) where n is the length of `nums`.
This comes two passes through `nums` and a final summation pass to construct
the result. The space complexity is also O(n) for the `prefix` and `suffix`
arrays.

```java
class Solution {
    public int[] getSumAbsoluteDifferences(int[] nums) {
        int len = nums.length;
        int[] prefix = new int[len];
        for (int i = 1; i < len; i++) {
            prefix[i] = prefix[i - 1] + nums[i - 1];
        }

        int[] suffix = new int[len];
        for (int j = len - 2; j > -1; j--) {
            suffix[j] = suffix[j + 1] + nums[j + 1];
        }

        int[] result = new int[len];
        for (int i = 0; i < len; i++) {
            // result[i] = -((len - (i + 1)) * nums[i] - suffix[i]) + ((i * nums[i]) - prefix[i]);
            // simplified
            result[i] = nums[i] * (-len + 1 + 2 * i) + suffix[i] - prefix[i];
        }
        return result;
    }
}
```
