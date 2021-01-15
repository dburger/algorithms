# Next Permutation

Implement **next permutation**, which rearranges numbers into the
lexicographically next greater permutation of numbers.

If such an arrangement is not possible, it must rearrange it as the lowest
possible order (i.e., sorted in ascending order).

The replacement must be in place and use only constant extra memory.

## Hints

1. Think of how you would do this by hand and just simulate that.

## Solutions

### Simulate the standard

Simulating the standard, look for the leftmost number that is less than
its immediate neighbor to the right. Say this is at position `swap` and
contains value `swapVal`. Then look for the smallest number to the right
of `swap` that is greater than `swapVal`. Swap these two numbers and then
sort everything to right of `swap`.

This solution has time complexity O(n * log(n)) due to the sort. The space
complexity is O(1).

```java
class Solution {
    public void nextPermutation(int[] nums) {
        // Look for swap position, if any. That is position of
        // left most digit lower than its neighbor to the immediate
        // right.
        int swap;
        for (swap = nums.length - 2; swap > -1; swap--) {
            if (nums[swap] < nums[swap + 1]) {
                break;
            }
        }

        // If we found a swap position, find the smallest digit to
        // the right that is greater than the digit at swap.
        if (swap > -1) {
            int swapVal = nums[swap];
            int swapWith = swap + 1;
            int swapWithVal = nums[swapWith];
            for (int i = swapWith + 1; i < nums.length; i++) {
                int val = nums[i];
                if (val > swapVal && val < swapWithVal) {
                    swapWith = i;
                    swapWithVal = val;
                }
            }
            // Swap them.
            nums[swap] = swapWithVal;
            nums[swapWith] = swapVal;
        }

        // Sort the digits to the right of swap in ascending order.
        Arrays.sort(nums, swap + 1, nums.length);
    }
}
```

### Simulate even faster

We can go faster! Note that the above solution has a time complexity of
O(n * log(n)) due to the sort that puts digits trailing the swap position
in order. We can do this without a sort. If we take care to swap with
the rightmost smallest digit that is is greater than `swapVal`, then instead
of sorting the trailing digits we can just reverse them.

This takes the time complexity down to O(n). The space complexity remains
O(1). In practice this does run faster on leeter.

```java
class Solution {
    public void nextPermutation(int[] nums) {
        // Look for swap position, if any. That is position of
        // left most digit lower than its neighbor to the immediate
        // right.
        int swap;
        for (swap = nums.length - 2; swap > -1; swap--) {
            if (nums[swap] < nums[swap + 1]) {
                break;
            }
        }

        // If we found a swap position, find the rightmost smallest digit to
        // the right that is greater than the digit at swap.
        if (swap > -1) {
            int swapVal = nums[swap];
            int swapWith = swap + 1;
            int swapWithVal = nums[swapWith];
            for (int i = swapWith + 1; i < nums.length; i++) {
                int val = nums[i];
                if (val > swapVal && val <= swapWithVal) {
                    swapWith = i;
                    swapWithVal = val;
                }
            }
            // Swap them.
            nums[swap] = swapWithVal;
            nums[swapWith] = swapVal;
        }

        // Sort the digits to the right of swap in ascending order.
        // Arrays.sort(nums, swap + 1, nums.length);
        int first = swap + 1;
        int last = nums.length - 1;
        int mid = first + (last - first + 1) / 2;
        for (int i = 0; first + i < mid; i++) {
            int temp = nums[last - i];
            nums[last - i] = nums[first + i];
            nums[first + i] = temp;
        }
    }
}
```
