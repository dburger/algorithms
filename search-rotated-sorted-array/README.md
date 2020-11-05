# Search in Rotated Sorted Array

You are given an integer array `nums` sorted in ascending order, and an integer
`target`.

Suppose that `nums` is rotated at some pivot unknown to you beforehand
(i.i, `[0,1,2,4,5,6,7]` might become `[4,5,6,7,0,1,2]`.

If `target` is found in the array return its index, otherwise return `-1`.

## Hints

1. Obviously brute force can just scan through and find target. The point of
   this question is to do better than that.
1. The rotation introduces two separate sorted segements. Figure out how to
   binary search on the correct segment.

## Solutions

### Binary searching the correct segment

```java
class Solution {

    public int search(int[] nums, int target) {
        // First locate the rotate point, the smallest element.
        int i = findSmallest(nums, 0, nums.length - 1);
        // Now we essentially have two separate sorted arrays.
        // Binary search on the correct half depending on where target lies.
        if (target >= nums[i] && target <= nums[nums.length - 1]) {
            return binSearch(nums, target, i, nums.length - 1);
        } else {
            return binSearch(nums, target, 0, i - 1);
        }
    }

    private int binSearch(int[] nums, int target, int lo, int hi) {
        if (lo > hi) {
            return -1;
        }
        int mid = lo + (hi - lo) / 2;
        if (target > nums[mid]) {
            return binSearch(nums, target, mid + 1, hi);
        } else if (target < nums[mid]) {
            return binSearch(nums, target, lo, mid - 1);
        } else {
            return mid;
        }
    }

    private int findSmallest(int[] nums, int lo, int hi) {
        int mid = lo + (hi - lo) / 2;
        if (nums[mid] > nums[hi]) {
            // Value at mid greater than hi, rotate point is to right.
            return findSmallest(nums, mid + 1, hi);
        } else if (nums[mid] < nums[hi]) {
            // Value at mid less than hi, rotate is this element or to left.
            return findSmallest(nums, lo, mid);
        } else {
            return mid;
        }
    }
}
```
