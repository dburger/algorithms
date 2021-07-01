# Remove Duplicates from Sorted Array II

Given an integer array `nums` sorted in **non-decreasing order**, remove some
duplicates in-place such that each unique element appears **at most twice**.
The **relative order** of the elements should be kept the **same**.

Since it is not possible to change the length of the array in some languages,
you must instead have the result be placed in the **first part** of the array
`nums`. More formally, if there are `k` elements after removing the duplicates,
then first first `k` elements of `nums` should hold the final result. It does
not matter what you leave beyond the first `k` elements.

Return `k` after placing the final result in the first `k` slots of `nums`.

Do not allocate extra space for another array. You must do this by **modifying
the input array in-place with O(1) extra memory.

## Hints

1. This is a slight variation on
   [Remove Duplicates from Sorted Array](../remove-duplicates-sorted-array).
1. This problem can be attacked with a common technique of keeping track of a
   `fill` position.

## Solutions

### Advancing fill

This approach keeps track of a `fill` position and how many consecutive
elements with the same value have been seen. When this value is under `3` it
fills. When it is greater than or equal to `3` it ignores.

The time complexity for this algorithm in O(n). The space complexity in O(1).

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        int consecutive = 1;
        int prior = nums[0];
        int fill = 0;
        for (int i = 1; i < nums.length; i++) {
            int curr = nums[i];
            if (curr == prior) {
                consecutive++;
            } else {
                consecutive = 1;
            }
            if (consecutive < 3) {
                nums[++fill] = curr;
            }
            prior = curr;
        }
        return fill + 1;
    }
}
```
