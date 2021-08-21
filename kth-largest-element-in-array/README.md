# Kth Largest Element in an Array

Given an integer array `nums` and an integer `k`, return the `k`th largest
element in the array.

Note that it is the `k`th largest element in the sorted order, not the `k`th
distinct element.

## Hints

1. Obviously sorting the array and picking the `k`th largest element would do
   the trick. This would be O(n * log(n)) to do the sort. We can do better.
1. This is one of the standard foundational algorithm problems of computer
   science. Instead of sorting we can repeatedly partition (one of the key
   steps of quicksort) until we have partitioned at the `k`th largest element.

## Solutions

### Classic partition

Here we show the classic partition approach. This `partition` method here
could be used to easily build out a full quicksort. Here, however, we only
need to partition in the direction of the `k`th largest element. Thus we
only have to recurse on one side of each pivot, not both as quicksort would
require.

The time complexity of this solution is O(n) in the average case. The space
complexity of this solution is O(1).

```java
class Solution {
    private final Random r = new Random();

    public int findKthLargest(int[] nums, int k) {
        int lo = 0;
        int hi = nums.length - 1;
        int target = nums.length - k;
        int pivot = partition(nums, lo, hi);
        while (pivot != target) {
            if (pivot < target) {
                lo = pivot + 1;
            } else {
                hi = pivot - 1;
            }
            pivot = partition(nums, lo, hi);
        }

        return nums[target];
    }

    private int partition(int[] nums, int lo, int hi) {
        if (lo == hi) {
            return lo;
        }
        // Random pivot to avoid problem with degenerate already sorted case.
        int pivot = r.nextInt(hi - lo) + lo;
        int pval = nums[pivot];
        swap(nums, pivot, hi);

        int fill = lo;
        for (int i = lo; i < hi; i++) {
            if (nums[i] < pval) {
                swap(nums, fill++, i);
            }
        }

        swap(nums, fill, hi);

        return fill;
    }

    private void swap(int[] nums, int i, int j) {
        int temp = nums[j];
        nums[j] = nums[i];
        nums[i] = temp;
    }
}
```
