# Minimum Size Subarray Sum

Given an array of positive integers `nums` and a positive integer `target`,
return the minimal length of a **contiguous subarray**
`[nums[i], nums[i + 1], ..., nums[r]]` of which the sum is greater than or
equal to `target`. If there is no such subarray, return `0` instead.

## Hints

1. Brute force will pass the leeter grader.
1. Windowing?

## Solutions

### Brute force

With the brute force approach we simply consider each element and see the
shortest run that leads to the `target` from that element. A small included
optimization exits if a run of length `1` is found, which can't be beaten.

The time complexity of this algorithm is O(n^2). This comes from the nested
loops iterating over the elements. The space complexity of this approach is
O(1).

```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int min = Integer.MAX_VALUE;
        for (int i = 0; i < nums.length; i++) {
            int sum = 0;
            for (int j = i; j < nums.length; j++) {
                sum += nums[j];
                if (sum >= target) {
                    min = Math.min(min, j - i + 1);
                    if (min == 1) {
                        return 1;
                    }
                }
            }
        }
        return min == Integer.MAX_VALUE ? 0 : min;
    }
}
```

### Brute force with tweakage

I noticed that special casing a single element beating `target` seemed to
increase the running speed quite a bit. Apparently initializing the second
loop is expensive?

Still, time complexity O(n^2) and space complexity O(1).

```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int min = Integer.MAX_VALUE;
        for (int i = 0; i < nums.length; i++) {
            int span = nums[i];
            if (span >= target) {
                return 1;
            }
            for (int j = i + 1; j < nums.length; j++) {
                span += nums[j];
                if (span >= target) {
                    min = Math.min(min, j - i + 1);
                    break;
                }
            }
        }
        return min == Integer.MAX_VALUE ? 0 : min;
    }
}
```

### Sliding window expand and clamp

This approach expands a window right index `j` until it becomes `>= target`.
Then it proceeds by moving the left index `i` as far as possible and still hit
that target. When it can't, it proceeds in moving the right index again. At
all times the `sum` is kept current by subtracting at the value that was at
`i` or adding in the new value coming from `j`.

The time complexity of this algorithm is O(n). This comes from either `i` or
`j` advancing on each iteration and `i` never passing `j` and the algorithm
terminating when `j` has traversed the length of the input. The space
complexity is O(1) as only a constant number of temp variables are cooked up
to track the solution.

```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int i = 0;
        int j = 0;
        int min = Integer.MAX_VALUE;
        int sum = nums[0];
        while (j < nums.length) {
            if (sum >= target) {
                min = Math.min(min, j - i + 1);
                if (min == 1) {
                    return 1;
                }
                sum -= nums[i++];
            } else {
                j++;
                if (j == nums.length) {
                    break;
                }
                sum += nums[j];
            }
        }
        return min == Integer.MAX_VALUE ? 0 : min;
    }
}
```
