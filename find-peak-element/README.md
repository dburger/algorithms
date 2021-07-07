# Find Peak Element

## Hints

1. It is possible to binary search for a peak by comparing the value at `mid`
   to the value to its right. How would you halve based upon this comparison?

## Solutions

### Recursive binary searching baby

Here we implement the solution alluded to by the hint above. Say we have
value `val` at `mid` and value `rval` at `mid + 1`. If `rval` is greater, then
we know there is a peak to the right. If `rval` is not greater, then we know
the peak is at `mid` or to the left.

One obvious question is how do we always know there will be a position
`mid + 1`? This comes from the terminating case, `mid` is always at least 1
less than then end position in the array. This comes from the rounding down
of integer division.

This solution has time complexity O(log(n)) and space complexity O(log(n)).
The time complexity comes from the halving nature of binary search. The space
complexity is the result of using a recursive approach. The stack frames may
reach a depth of O(log(n)) to arrive at a solution.

```java
class Solution {
    public int findPeakElement(int[] nums) {
        return fpe(nums, 0, nums.length - 1);
    }

    private static int fpe(int[] nums, int lo, int hi) {
        if (lo == hi) {
            return lo;
        }
        int mid = lo + (hi - lo) / 2;
        int val = nums[mid];
        int rval = nums[mid + 1];
        return val < rval ? fpe(nums, mid + 1, hi) : fpe(nums, lo, mid);
    }
}
```

### Iterative binary searching baby

OK, not liking the space complexity of the prior solution, let's use an
iterative binary search to change that to O(1).

The time complexity remains O(log(n)).

```java
class Solution {
    public int findPeakElement(int[] nums) {
        int lo = 0;
        int hi = nums.length - 1;
        while (lo < hi) {
            int mid = lo + (hi - lo) / 2;
            int val = nums[mid];
            int rval = nums[mid + 1];
            if (val < rval) {
                lo = mid + 1;
            } else {
                hi = mid;
            }
        }
        return lo;
    }
}
```
