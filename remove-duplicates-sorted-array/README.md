# Remove Duplicates from Sorted Array

Given a sorted array `nums`, remove the duplicates in-place such that each
element appears only once and returns the new length.

Do not allocate space for another array, you must do this by **modifying the
input array** in-place with O(1) extra memory.

## Hints

1. The specification of sorted makes it so that you only need to look at
   neighbors.
1. In-place adds a twist to the problem. If this were not specified you could
   allocate a new array and copy into it when next was not the same as prior.
   With in-place you can proceed in the same way but you must keep track of
   where to fill.

## Solutions

### Brute force but violating, part I

Here I present a brute force solution that works but violates the problem
constraints. Namely, it violates the "Do not allocate space for another
array..." part.

This solution has a time complexity of O(n) where n is the length of the input
array as it iterates through the array up to twice. The space complexity of
this solution is also O(n), as an array is allocated to hold the result before
it is copied back into `nums`.

```java
class Solution {
    public int[] removeDuplicates(int nums[]) {
        List<Integer> result = new ArrayList<>();
        for (int i = 0; i < nums.length; i++) {
            int val = nums[i];
            int size = result.size();
            if (size == 0 || val != result.get(size - 1)) {
                result.add(val);
            }
        }
        for (int i = 0; i < result.size(); i++) {
            nums[i] = result.get(i);
        }
        return result.size();
    }
}
```

### Brute force but violating, part II

This is the same solution as above with a minor twist. Here instead of looking
at the last element in `result` to identify duplicates, we look at the prior
element in `nums`.

```java
class Solution {
    public int[] removeDuplicates(int nums[]) {
        List<Integer> result = new ArrayList<>();
        for (int i = 0; i < nums.length; i++) {
            int val = nums[i];
            if (result.size() == 0 || val != nums[i - 1]) {
                result.add(val);
            }
        }
        for (int i = 0; i < result.size(); i++) {
            nums[i] = result.get(i);
        }
        return result.size();
    }
}
```

### Fill and iterate

To do this problem in place we use a common technique in this type of array
problem. That is, we keep track of a `filled` position, starting at `0`.
Position `0` is obviously always correct. Then, as we iterate across starting
at position `1`, if a number is the same as its prior we do nothing. If it is
not the same we increment `filled` and fill that position. Note the guard
clause at the top that checks for special cases of input `nums` of length
less than 2 where no work needs to be done.

The time complexity of this solution is O(n), the size of the input array,
as we must examine each value in the array. The time complexity for this
problem is O(1) as required by the problem statement. That is we only allocate
a `filled` variable for tracking regardless of the input size.

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        if (nums.length == 0 || nums.length == 1) {
            return nums.length;
        }
        int filled = 0;
        for (int i = 1; i < nums.length; i++) {
            if (nums[i] != nums[i - 1]) {
                nums[++filled] = nums[i];
            }
        }
        return filled + 1;
    }
}
```
