# Median of Two Sorted Arrays

Given two sorted arrays `nums1` and `nums2` of size `m` and `n` respectively,
return **the median** of the two sorted arrays.

## Hints

1. Since these are already sorted a brute force approach could merge the
   arrays in O(m) if say, `m` is the length of the longer array, and then
   merely pick the median.

## Solutions

### Brute force

Merge the sorted arrays and pick the median. This is O(m) if `m` is the length
of the longer array.

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int nums1Len = nums1.length;
        int nums2Len = nums2.length;
        int mergedLen = nums1Len + nums2Len;
        int[] merged = new int[mergedLen];
        int i = 0;
        int j = 0;
        int fill = 0;
        while (i < nums1Len || j < nums2Len) {
            if (i < nums1Len && j < nums2Len) {
                int val1 = nums1[i];
                int val2 = nums2[j];
                if (val1 <= val2) {
                    merged[fill] = val1;
                    i++;
                } else {
                    merged[fill] = val2;
                    j++;
                }
            } else if (i < nums1Len) {
                merged[fill] = nums1[i++];
            } else {
                merged[fill] = nums2[j++];
            }
            fill++;
        }
        if (mergedLen % 2 == 1) {
            return merged[(nums1Len + nums2Len) / 2];
        } else {
            int mid = mergedLen / 2;
            return (merged[mid] + merged[mid - 1]) / 2.0;
        }
    }
}
```
