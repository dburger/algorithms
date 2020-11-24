# Find Minimum in Rotated Sorted Array

Suppose an array of length `n` sorted in ascending order is **rotated**
between `1` and `n` times. For example, the array `nums = [0,1,2,3,4,5,6,7]`
might become:

* `[4,5,6,7,0,1,2]` if it were rotated `4` times
* `[0,1,2,4,5,6,7]` if it were rotated `7` times

Notice that **rotating** an array `[a[0], a[1], a[2], ..., a[n-1]]` 1 time
results in the array `[a[n-1], a[0], a[1], a[2], ..., a[n-2]]`.

Given the sorted rotated array `nums`, return the *minimum element of this
array*.

## Hints

1. A modified binary search can do what you want. Instead of looking for the
   match, you look for whether or not the relationship between the `mid` and
   `right` elements is as expected.

## Solutions

### Modified binary search

This binary search is modified to look at whether or not the element at
`mid` is greater than the element at `right`. If it is, the minimum element
is to the right. If it is not, the minimum element is either at `mid` or
to the left.

```java
class Solution {
    public int findMin(int[] nums) {
        int n = nums.length;
        int left = 0;
        int right = n - 1;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] > nums[right]) {
                // minimum is to the right
                left = mid + 1;
            } else {
                // minimum is at mid or to the left
                right = mid;
            }
        }
        return nums[left];
    }
}
```
