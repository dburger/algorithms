# Find Pivot Index

Given an array of integers `nums`, write a method that returns the "pivot" index
of this array.

We define the **pivot index** as the index where the sum of all the numbers to
the left of the index is equal to the sum of all the numbers to the right of the
index.

If no such index exists, we should return -1. If there are multiple pivot
indexes, you should return the left-most pivot index.

## Hints

1. Just choose the left most position as the index and keep shifting until you
   find an answer or run off the edge of the array.

## Solutions

### Shift and check

This is a cool little problem. The implementation is simple, with the caveat
that you need to get your indices and increments just right. Be careful.

This solution is O(n) as you iterate the array fully once and may need to
iterate it up to twice.

```java
class Solution {
    public int pivotIndex(int[] nums) {
        if (nums.length == 0) {
            return -1;
        }
        int pivot = 0;
        int leftSum = 0;
        int rightSum = 0;
        for (int i = 1; i < nums.length; i++) {
            rightSum += nums[i];
        }
        while (leftSum != rightSum && pivot < nums.length - 1) {
            leftSum += nums[pivot++];
            rightSum -= nums[pivot];
        }
        return leftSum == rightSum ? pivot : -1;
    }
}
```
