# Sort an Array

Given an array of integers `nums`, sort the array in ascending order.

## Hints

1. Well it is a sort, remember your sorts?
1. While it is a sort, read the constraints!

## Solutions

### Yort snajort quicksort!

This solution is a quicksort implementation. It works by partioning elements
lower than or equal to and higher than the pivot. Then recursion operates on
both sides of the partition.

The time complexity of this solution depends on the selection of the pivot.
If the pivot ends up dividing the array pieces in half then the time complexity
is O(n * log(n)) and the space complexity is O(log(n)) to handle the recursion.
In degenerate cases where the pivot does not divide the array pieces the
time complexity is O(n^2) and space complexity is O(n).

```
class Solution {
    public int[] sortArray(int[] nums) {
        if (nums.length < 2) {
            return nums;
        }

        sort(nums, 0, nums.length - 1);
        return nums;
    }

    private void sort(int[] nums, int lo, int hi) {
        if (lo >= hi) {
            return;
        }

        int p = partition(nums, lo, hi);
        sort(nums, lo, p - 1);
        sort(nums, p + 1, hi);
    }

    int partition(int[] nums, int lo, int hi) {
        // Should randomize the pivot to avoid degenerate cases.
        int pval = nums[lo];
        int fill = lo + 1;
        for (int i = fill; i <= hi; i++) {
            int val = nums[i];
            if (val <= pval) {
                int temp = nums[i];
                nums[i] = nums[fill];
                nums[fill] = temp;
                fill++;
            }
        }

        fill--;
        nums[lo] = nums[fill];
        nums[fill] = pval;
        return fill;
    }
}
```

### Constraints yield buckets

Looking at the constraints we can see that the possible values are limited
to [-5 * 10^4, 5 * 10^4]. Since this is an integer array size we can easily
allocate we can do this problem with a bucket sort. Here we initialize an
array, count into the buckets, and then fill back into the input array.

Using bucket sort the time and space complexity are O(1). The space used is
constant regardless of the input.

```java
class Solution {
    public int[] sortArray(int[] nums) {
        // Used to translate [-offset, offset] into [0, 2 * offset + 1].
        int offset = 5 * 10000;
        int[] buckets = new int[2 * offset + 1];
        for (int n : nums) {
            buckets[n + offset]++;
        }

        int fill = 0;
        for (int i = 0; i < buckets.length; i++) {
            int count = buckets[i];
            while (count-- > 0) {
                nums[fill++] = i - offset;
            }
        }
        return nums;
    }
}
```
