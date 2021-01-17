# Get Maximum in Generated Array

You are given an integer `n`. An array `nums` of length `n + 1` is generated in
the following way:

* `nums[0] = 0`
* `nums[1] = 1`
* `nums[2 * i] = nums[i]` when `2 <= 2 * i <= n`
* `nums[2 * i + 1] = nums[i] + nums[i + 1]` when `2 <= 2 * i + 1 <= n`

Return the **maximum** integer in the array `nums`.

## Hints

1. A literal translation would seem to work.

## Solutions

### Literal translation

This is a literal translation of the problem description. Pay note to the
selection of the starting point for `i` and the continuation constrains in the
primary loop.

This solution has O(n) time and space complexity.

```java
class Solution {
    public int getMaximumGenerated(int n) {
        if (n < 2) {
            return n;
        }
        int[] nums = new int[n + 1];
        nums[0] = 0;
        nums[1] = 1;
        for (int i = 1; 2 * i + 1 <= n; i++) {
            nums[2 * i] = nums[i];
            nums[2 * i + 1] = nums[i] + nums[i + 1];
        }
        int max = Integer.MIN_VALUE;
        for (int i = 0; i < n + 1; i++) {
            int val = nums[i];
            if (val > max) {
                max = val;
            }
        }
        return max;
    }
}
```
