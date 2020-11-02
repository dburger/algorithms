# Find all Duplicates in an Array

Given an array of integers, 1 <= a[i] <= n (n = size of array), some
elements appear **twice** and others appear **once**. Find all the
elements that appear twice in this array.

Could you do it without extra space and in O(n) runtime?

## Hints

1. A brute force solution could use nested loops to look for duplicates.
1. A better solution could use a set to look for duplicates.
1. The question at the bottom of the problem is the solution you should be
   trying to accomplish. The contstraints on the contents of the array
   make it possible.

## Solutions

### Marking with negatives

Because the values are constrained to [1, n] where n is the size of the
array:

1. The values can be converted to indexes back into the array.
2. Negative can be used to mark a seen value.

This gives O(n) runtime with no extra space.

```java
class Solution {
    public List<Integer> findDuplicates(int[] nums) {
        List<Integer> result = new ArrayList<>();
        for (int i = 0; i < nums.length; i++) {
            // Take absolute value in case this position is already marked.
            int val = Math.abs(nums[i]);
            int markVal = nums[val - 1];
            if (markVal < 0) {
                // Was marked negative already, this has been seen.
                result.add(val);
            } else {
                // Mark it as has been seen.
                nums[val - 1] = -nums[val - 1];
            }
        }
        return result;
    }
}
```
