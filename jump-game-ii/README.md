# Jump Game II

Given an array of non-negative integers `nums`, you are initially positioned
at the first index of the array. Each element represents the maximum jump
length at that position.

Your goal is to reach the last index in the minimum number of jumps.

You can assume you can always reach the last index.

## Hints

## Solutions

### Brute force

Here we merely keep a mins array and then track the minimum number of hops
to get to each position in the array. A value of `0` for all positions other
than the first position indicates that the position has not been initialized
yet.

The time complexity of this algorithm is O(n^2). This is because of the nested
loops where the inner loop could iterate from the current position to the end
of the array. The space complexity of this algorithm is O(n). This is because
of the auxillary array `mins` used to hold the minimum jumps to each position.

Oddly, this algorithm seems to finish is 0 ms on the leeter. One would think
that only O(n) solutions would do so.

```java
class Solution {
    public int jump(int[] nums) {
        int[] mins = new int[nums.length];
        for (int i = 0; i < mins.length; i++) {
            int maxHop = nums[i];
            for (int j = 1; j <= maxHop && i + j < mins.length; j++) {
                // If not initialized or better than previous value.
                if (mins[i + j] == 0 || mins[i] + 1 < mins[i + j]) {
                    mins[i + j] = mins[i] + 1;
                }
            }
        }
        return mins[mins.length - 1];
    }
}
```

### Current and max reach tracking

Here we keep track of the maxReach and the current position at or before that
we would need to jump.

This solution has time complexity O(n) and space complexity O(1).

```java
class Solution {
    public int jump(int[] nums) {
        int n = nums.length;
        int jumps = 0;
        int current = 0;
        int maxReach = 0;

        for (int i = 0; i < n - 1; i ++) {
            // Keep track of furthest we can get to.
            maxReach = Math.max(maxReach, i + nums[i]);
            if (i == current) {
                // Would need to jump at or before current.
                jumps++;
                current = maxReach;
            }
        }
    }
}
```
