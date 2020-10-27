# Maximum Subarray

Given an integer array `nums`, find the contiguous subarray (containing at least
one number) which has the largest sum and return *its sum*.

## Hints

1. Keep variables for max and current, updating as needed.

## Solutions

### Kadane's algorithm solution

This solution is based on
[Kadane's algorithm](https://en.wikipedia.org/wiki/Maximum_subarray_problem#Kadane's_algorithm).

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int current = 0;
        int max = Integer.MIN_VALUE;
        for (int n : nums) {
            current += n;
            max = Math.max(current, max);
            // If n took current negative, throw it away and start over.
            current = Math.max(0, current);
        }
        return max;
    }
}
```
