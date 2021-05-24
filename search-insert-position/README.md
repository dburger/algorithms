# Search Insert Position

Given a sorted array of distinct integers and a target value, return the index if the target is
found. If not, return the index where it would be if it were inserted in order.

You must write an algorithm with `O(log n)` runtime complexity.

## Hints

1. Brute force is dead simple.
1. A modified binary search could do the trick.

## Solutions

### Brute force

The brute force is dead simple but it does not satisfy the problem constraint of being an
`O(log n)` solution. Note that the leeter grader won't fail this solution even with this O(n)
approach being used. As a matter of fact, for the test cases provided this algorithm will also
clock in at 0 ms.

```java
class Solution {
    public int searchInsert(int[] nums, int target) {
      for (int i = 0; i < nums.length; i++) {
          if (nums[i] >= target) {
              return i;
          }
      }
      return nums.length;
    }
}
```

### Binary search and check

This solution follows the hint by modifying binary search. How does binary search need to be
modified. Well, obviously instead of returning a `-1` on not found we want to return the position
to insert. Here we make such a modification. Binary search is modified to loop while `lo < hi`.
If it finds the `target`, the correct position is returned. If it does not find the `target` then
after the loop then we examine `lo`. If the value at `lo` is larger than `target` then we can
insert at `lo`. If the value at `lo` is smaller than `target` then we can insert at `lo + 1`.

This solution satisfies the problem constraints by having a time complexity of O(log n). The
space complexity is O(1) as we have used an iterative binary search here.

```java
class Solution {
    public int searchInsert(int[] nums, int target) {
        return bs(nums, target);
    }

    private int bs(int[] nums, int target) {
        int lo = 0;
        int hi = nums.length - 1;
        while (lo < hi) {
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
        return nums[lo] < target ? lo + 1 : lo;
    }
}
```
