# Subarray Sum Equals K

Given an array of integers `nums` and an integer `k`, return the total number
of continuous subarrays whose sum equals to `k`.

## Hints

1. Note that in this problem the array elements may be negative. This makes
   sliding window approaches not work.

## Solutions

### Brute force

This solution is the brute force solution which computes the sum for each
continuous subarray within `nums`.

This solution has O(n^2) time complexity and O(1) space complexity. The time
complexity comes from the nested loops, where the outer loop executes `n`
times where `n` is the length of `nums`. The inner loop executes up to `n`
times thus the O(n^2) bound.

```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        int count = 0;
        for (int i = 0; i < nums.length; i++) {
            int sum = 0;
            for (int j = i; j < nums.length; j++) {
                sum += nums[j];
                if (sum == k) {
                    count++;
                }
            }
        }
        return count;
    }
}
```

### Map and check

This clever solution uses a map to keep track of the cumulative sum for each
position in the array along with a count for how many times that cumulative
sum has been seen. For each additional cumulative sum going to the right, we
check if the map has an entry for the current cumulative sum minus target `k`.
To make this more explicit let's call the current cumulative sum `sumc` and
some prior cumulative sum `sump`. If the subtraction `sumc - k == sump` this
means that the elements from the one after `sump` up to and including `sumc`
sum to k. That is, solving the equation for k, `sumc - sump == k`.

This solution has O(n) time complexity and O(n) space complexity. The time
complexity comes from the single pass through the array to compute and map the
cumulative sums. The space complexity stems from the mapping having perhaps
an entry for each entry of `nums`, that is the cumulative sums are all
different.

```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        int result = 0;
        int sum = 0;
        Map<Integer, Integer> map = new HashMap<>();
        map.put(0, 1);
        for (int n : nums) {
            sum += n;
            result += map.getOrDefault(sum - k, 0);
            map.put(sum, map.getOrDefault(sum, 0) + 1);
        }
        return result;
    }
}
```
