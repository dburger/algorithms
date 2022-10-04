# Move Zeroes

Given an integer array `nums`, move all `0`'s to the end of it while
maintaining the relative order of the non-zero elements.

## Hints

1. Use your `fill` approach.

## Solutions

### Phil, can we find `fill`

This solution merely keeps track of a `fill` position that it uses
for non-zero elements. When the end of the array is reached, zeroes
are placed from the current `fill` to the end of the array.

The time complexity of this approach is O(n) where `n` is the length
of the array. The space complexity is O(1).

```java
class Solution {
    public void moveZeroes(int[] nums) {
        int fill = 0;
        for (int n : nums) {
            if (n != 0) {
                nums[fill++] = n;
            }
        }
        while (fill < nums.length) {
            nums[fill++] = 0;
        }
    }
}
```
