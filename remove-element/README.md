# Remove Element

Given an array `nums` and a value `target`, remove all instances of that value in-place and
return the new length.

Do not allocate extra space for another array, you must do this by **modifying the input array**
in-place with `O(1)` extra memory.

The order of the elements can be changed. It doesn't matter what you leave beyond the new length.

## Hints

1. This can be solved with a common array technique of keeping track of a valid fill position.

## Solutions

### Fill me up

This solution follows the hint using a `fill` variable to keep track of the last valid fill
position. When we see an element that should remain in the array we increment fill and place it
there. The only confusing part of this algorithm is that until it finds the first element that
doesn't belong in the array, all of the work it is doing is actually just swapping elements with
themselves.

This solution has O(n) time complexity and O(1) space complexity.

```java
class Solution {
    public int removeElement(int[] nums, int target) {
        int fill = -1;
        for (int i = 0; i < nums.length; i++) {
            int val = nums[i];
            if (val != target) {
                nums[++fill] = val;
            }
        }
        return fill + 1;
    }
}
```
