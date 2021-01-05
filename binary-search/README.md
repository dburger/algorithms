# Binary Search

Given a **sorted** (in ascending order) integer array of `nums` of `n` elements
and a `target` value, write a function to search for `target` in `nums`. If
`target` exists, then return its index, otherwise return `-1`.

## Hints

1. This is of course a basic algorithm that all computer science folks should
   know. No hints here. OK, I'll give one below.
1. The usual approach, iterative or recursive, is to advance via `lo` and `hi`
   index indicators. Calculate the `mid`, look a the value at `mid`, and then
   adjust `lo` or `hi` accordingly.

## Solutions

### Recursive binary search

The time complexity is O(log(n)) where n is the length of `nums`, as the list
is halved on each invocation. The space complexity is O(log(n)), as the
recursion may need to go that deep.

```java
class Solution {
    public int search(int[] nums, int target) {
        return s(nums, target, 0, nums.length - 1);
    }

    public int s(int[] nums, int target, int lo, int hi) {
        if (lo > hi) return -1;
        int mid = lo + (hi - lo) / 2;
        int val = nums[mid];
        if (target > val) {
            return s(nums, target, mid + 1, hi);
        } else if (target < val) {
            return s(nums, target, lo, mid - 1);
        } else {
            return mid;
        }
    }
}
```

### Iterative binary search

The time complexity is O(log(n)) where n is the length of `nums`,
as the list is halved in each loop. The time complexity is O(1),
as only three trivial index trackers are used in the operation
of the algorithm.

```java
class Solution {
    public int search(int[] nums, int target) {
        int lo = 0;
        int hi = nums.length - 1;
        while (lo <= hi) {
            int mid = lo + (hi - lo) / 2;
            int val = nums[mid];
            if (target > val) {
                lo = mid + 1;
            } else if (target < val) {
                hi = mid - 1;
            } else {
                return mid;
            }
        }
        return -1;
    }
}
```
