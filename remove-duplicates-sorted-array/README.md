# Remove Duplicates from Sorted Array

Given a sorted array `nums`, remove the duplicates in-place such that each
element appears only once and returns the new length.

Do not allocate space for another array, you must do this by **modifying the
input array** in-place with O(1) extra memory.

## Hints

1. The specification of sorted makes it so that you only need to look at
   neighbors.
1. In-place adds a twist to the problem. If this were not specified you could
   allocate a new array and copy into it when next was not the same as prior.
   With in-place you can proceed in the same way but you must keep track of
   where to fill.

## Solutions

### Fill and iterate

To do this problem in place we use a common technique in this type of array
problem. That is, we keep track of a `filled` position, starting at `0`, and
when we see a next that doesn't match a prior we increment `filled` and fill
that value. Whenever we see a next that does match the prior that value has
already been filled and nothing needs to be done.

The time complexity of this solution is O(n), the size of the input array,
as we must examine each value in the array. The time complexity for this
problem is O(1) as required by the problem statement. That is we only allocate
a `filled` variable for tracking regardless of the input size.

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        if (nums.length == 0 || nums.length == 1) {
            return nums.length;
        }
        int filled = 0;
        for (int i = 1; i < nums.length; i++) {
            if (nums[i] != nums[i - 1]) {
                nums[++filled] = nums[i];
            }
        }
        return filled + 1;
    }
}
```
