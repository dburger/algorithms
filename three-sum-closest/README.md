# Three Sum Closest

Given an array `nums` of *n* integers and an integer `target`, find three
integers in `nums` such that the sum is closest to `target`. Return the sum of
the three integers. You may assume that each input would have exactly one
solution.

## Hints

1. You can work this like "Three Sum", looking for a closest instead of a
   match.

## Solutions

### Adjusted Three Sum

This solution coerces this problem to be like "Three Sum." The first step is
to sort the array which makes that part O(n*log(n)). Then the three sum loop
is used which is O(n^2). Thus the overall big O is O(n^2).

```java
class Solution {
    public int threeSumClosest(int[] nums, int target) {
        Arrays.sort(nums);
        int closest = 0;
        int diff = Integer.MAX_VALUE;
        for (int i = 0; i < nums.length - 2; i++) {
            int lo = i + 1;
            int hi = nums.length - 1;
            while (lo < hi) {
                int val = nums[i] + nums[lo] + nums[hi];
                int newdiff = Math.abs(target - val);
                if (newdiff < diff) {
                    diff = newdiff;
                    closest = val;
                }
                if (val < target) {
                    lo++;
                } else if (val > target) {
                    hi--;
                } else {
                    return target;
                }
            }
        }
        return closest;
    }
}
```
