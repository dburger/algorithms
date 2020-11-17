# Merge Sorted Array

Given two sorted integer arrays `nums1` and `nums2`, merge `nums2` into
`nums1` as one sorted array.

**Note**:

* The number of elements initialized in `nums1` and `nums2` are `m` and
  `n` respectively.
* You may assume that `nums1` has enough space (size that is **equal**
  to `m + n`) to hold additional elements from `nums2`.

## Hints

1. If you work forward you would have to shift elements of `nums1` out of
   the way. This would be messy and time consuming. Can you work backwards?

## Solutions

### Backwards fill in

By working from the back to the front, you can be assured that you don't have
to shift elements of `nums1` out of the way.

```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        int fill = m + n - 1;
        int pt1 = m - 1;
        int pt2 = n - 1;
        while (pt1 > -1 || pt2 > -1) {
            int val1 = pt1 > -1 ? nums1[pt1] : Integer.MIN_VALUE;
            int val2 = pt2 > -1 ? nums2[pt2] : Integer.MIN_VALUE;
            if (val1 >= val2) {
                nums1[fill--] = val1;
                pt1--;
            } else {
                nums1[fill--] = val2;
                pt2--;
            }
        }
    }
}
```
