# First Missing Positive

Given an unsorted integer array `nums`, find the smallest missing positive
integer.

This problem is a bit brutal with the details that must be known to answer this
problem:

* Can there be negative values? Yes.
* Can there be duplicate values? Yes.

## Hints

1. Sorting the array would be a good place to start.

## Solutions

### Sort and scan solution

This solution works by sorting the numbers and then scanning through to
determine the first missing positive. It requires O(n*log(n)) to sort the array.

Apparently there is also an O(n) solution but I have not figured that one out
yet.

```java
class Solution {
    public int firstMissingPositive(int[] nums) {
        if (nums.length == 0) {
            return 1;
        }
        int expected = 1;
        Arrays.sort(nums);
        for (int i = 0; i < nums.length; i++) {
            int value = nums[i];
            if (value < 1) {
                // We are looking for first missing positive,
                // if negative just continue.
                continue;
            } else if (i > 0 && value == nums[i - 1]) {
                // If duplicate, just continue.
                continue;
            }
            if (value != expected) {
                // If wasn't the exected value, return expected.
                return expected;
            }
            // Advance expected to next value.
            expected++;
        }

        // If we got through the end of the array without finding first missing
        // postive, then if the end of array is negative the first missing
        // positive is 1, else it is the last value + 1.
        int last = nums[nums.length - 1];
        return last <= 0 ? 1 : last + 1;
    }
}
```
