# Find First and Last Position of Element in Sorted Array

Given an array of integers `nums` sorted in ascending order, find the starting
and ending position of a given `target` value.

If `target` is not found in the array, return `[-1, -1]`.

You must write an algorithm with `O(log n)` runtime complexity.

## Hints

1. Brute force can easily be done a couple of different ways and is O(n).
1. Binary searching to the target and then expanding from there is also easy,
   however, expanding from target makes this also O(n).
1. Could binary search be adjusted to be able to return leftmost and rightmost?

## Solutions

### Brute force

The brute force approach has an O(n) time complexity and O(1) space complexity. This surprisingly
passes the leeter grader even though the problem states:

"You must write an algorithm with `O(log n)` runtime complexity."

Thus it is not a valid solution given the problem constraints.

```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int start = -1;
        int end = -1;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] == target) {
                if (start == -1) {
                    start = i;
                }
            } else if (start != -1 && end == -1) {
                end = i - 1;
                break;
            }
        }
        if (start != -1 && end == -1) {
            end = nums.length - 1;
        }
        return new int[] {start, end};
    }
}
```

### Binary search (recursive) and expand

From the second hint, this is the binary search and expand from there approach. Once again, this
is not a valid solution for this problem given the constraints per the time complexity as
described below. As in the prior problem, this one passes the leeter grader in spite of this
invalid time complexity.

This solution has a time complexity of O(n) and a space complexity of O(log(n)). The time
complexity comes from the expand step. It is possible that each item in the input array matches
target and thus the expand step will expand to both ends of the array. The space complexity
comes from the usage of a recursive binary search which could lead to a stack depth of log(n).

```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        if (nums.length == 0) {
            return new int[] {-1, -1};
        }
        int i = binSearch(nums, target, 0, nums.length - 1);
        if (i == - 1) {
            return new int[] {-1, -1};
        }
        int lo = i;
        int hi = i;
        while (lo > 0 && nums[lo - 1] == target) {
          lo--;
        }
        while (hi < nums.length - 1 && nums[hi + 1] == target) {
          hi++;
        }
        return new int[] {lo, hi};
    }

    private int binSearch(int[] nums, int target, int lo, int hi) {
        if (lo > hi) {
            return -1;
        }
        int mid = lo + (hi - lo) / 2;
        int val = nums[mid];
        if (target > val) {
            return binSearch(nums, target, mid + 1, hi);
        } else if (target < val) {
            return binSearch(nums, target, lo, mid - 1);
        } else {
            return mid;
        }
    }
}
```

### Binary search (iterative) and expand

This is the same solution as the prior with the recursive binary search replaced with an
iterative approach. This makes the space complexity for this solution O(1). The time complexity
remains O(n), due to the expand, as in the prior problem.

```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int lo = binSearch(nums, target);
        if (lo == -1) {
            return new int[] {-1, -1};
        }
        int hi = lo;
        while (lo > 0 && nums[lo - 1] == target) {
            lo--;
        }
        while (hi < nums.length - 1 && nums[hi + 1] == target) {
            hi++;
        }
        return new int[] {lo, hi};
    }

    private int binSearch(int[] nums, int target) {
        int lo = 0;
        int hi = nums.length - 1;
        while (lo <= hi) {
            int mid = lo + (hi - lo) / 2;
            int val = nums[mid];
            if (val < target) {
                lo = mid + 1;
            } else if (val > target) {
                hi = mid - 1;
            } else {
                return mid;
            }
        }
        return -1;
    }
}
```

### Modified binary search

This approach modifies the binary search to be able to return the
leftmost and rightmost occurrences. By doing two binary searches
this is a 2*O(log n) approach which is O(log n).

```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        if (nums.length == 0) {
            return new int[] {-1, -1};
        }
        int lo = binSearch(nums, target, 0, nums.length, true);
        if (lo == nums.length || nums[lo] != target) {
            return new int[] {-1, -1};
        }
        // Found lo on left, this right result will be one higher than
        // actual value.
        int hi = binSearch(nums, target, 0, nums.length, false) - 1;
        return new int[] {lo, hi};
    }

    private int binSearch(int[] nums, int target, int lo, int hi, boolean left) {
        if (lo == hi) {
            return lo;
        }
        int mid = lo + (hi - lo) / 2;
        int val = nums[mid];
        if (val > target || (left && target == val)) {
            // Note the recursion on the left side when the target matches
            // that includes the match (mid).
            return binSearch(nums, target, lo, mid, left);
        } else {
            // Note that when matches but left is false, recursion on the
            // right side. Thus when left is false will return index one
            // higher than rightmost.
            return binSearch(nums, target, mid + 1, hi, left);
        }
    }
}
```
