# Median of Two Sorted Arrays

Given two sorted arrays `nums1` and `nums2` of size `m` and `n` respectively,
return **the median** of the two sorted arrays.

## Hints

1. Since these are already sorted a brute force approach could merge the
   arrays in O(m) if say, `m` is the length of the longer array, and then
   merely pick the median.

## Solutions

### Brute force

Merge the sorted arrays and pick the median. This is O(max(m, n)).

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

### Binary search to the partition point

This binary searches to the partition point between nums1 and nums2 that results
in indexes i and j respectively that achieve:

nums1[i - 1] < nums2[j]
nums2[j - 1] < nums1[i]

This is O(log(min(m, n))).

```java
class Solution {
    public static double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int m = nums1.length;
        int n = nums2.length;
        if (m > n) {
            int[] tmpa = nums1;
            nums1 = nums2;
            nums2 = tmpa;
            int tmp = m;
            m = n;
            n = tmp;
        }
        int iMin = 0, iMax = m, halfLen = (m + n + 1) / 2;
        while (iMin <= iMax) {
            int i = (iMin + iMax) / 2;
            int j = halfLen - i;
            if (i < iMax && nums2[j - 1] > nums1[i]) {
                iMin = i + 1;
            } else if (i > iMin && nums1[i - 1] > nums2[j]) {
                iMax = i - 1;
            } else {
                int maxLeft = 0;
                if (i == 0) {
                    maxLeft = nums2[j - 1];
                } else if (j == 0) {
                    maxLeft = nums1[i - 1];
                } else {
                    maxLeft = Math.max(nums1[i - 1], nums2[j - 1]);
                }
                if ((m + n) % 2 == 1) {
                    return maxLeft;
                }

                int minRight = 0;
                if (i == m ) {
                    minRight = nums2[j];
                } else if (j == n) {
                    minRight = nums1[i];
                } else {
                    minRight = Math.min(nums2[j], nums1[i]);
                }
                return (maxLeft + minRight) / 2.0;
            }
        }
        return 0.0;
    }
}
```
