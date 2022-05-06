# Target Sum

You are given an integer array `nums` and an integer `target`.

You want to build an **expression** out of nums by adding one of the symbols
`'+'` and `'-'` before each integer in nums and then concatenate all the
integers.

- For example, if `nums = [2, 1]`, you can add a `'+'` before `2` and a `'-'`
before `1` and concatenate them to build the expression `"+2-12"`.

Return the number of different **expressions** that you can build, which
evaluates to `target`.

## Hints

1. Brute force recursion could build the full tree to count the result?
1. How would you use dynamic programming to do this? What is the table for a
   table approach?

## Solutions

### Brute me

This solution uses brute force to calculate each possible sum and see if it
matches `target`. This results in a pattern that is much like a backtracking
problem but there is no backtracking here.

The time complexity of this solution is O(2^n). This comes from having to
create the full branching tree. The space complexity is O(n), the depth of the
tree.

```java
class Solution {
    public int findTargetSumWays(int[] nums, int target) {
        int[] count = new int[1];
        ftsw(nums, 0, 0, target, count);
        return count[0];
    }

    private void ftsw(int[] nums, int pos, int sum, int target, int[] count) {
        if (pos == nums.length) {
            if (sum == target) {
                count[0]++;
            }
            return;
        }

        ftsw(nums, pos + 1, sum + nums[pos], target, count);
        ftsw(nums, pos + 1, sum - nums[pos], target, count);
    }
}
```

### Dynamically off the table

This problem yields nicely to a dynamic programming solution. How do you build
the table? The key insight comes looking at what the maximum positive and
negative value could be coming out of the sum. If we take the total of all the
entries the highest value we can get is `total` and the lowest we can get is
`-total`. Thus we can build a table where each row `i` counts the values that
can be acheived up to position `i` of the input.

The time complexity of this solution is O(t * n) where `t` is the total and
`n` is the number of entries. This comes from iterating over the full table.
Likewise, the space complexity is also O(t * n) to hold the table.

This solution has a bunch of places to make mistakes! Most of this stems from
translating `[-total, +total]` into the array indexed by `[0, 2 * total + 1]`.
Be careful!

```java
class Solution {
    public int findTargetSumWays(int[] nums, int target) {
        int total = 0;
        for (int val : nums) {
            total += val;
        }

        // Did they give us a target that isn't acheivable?
        if (Math.abs(target) > total) {
            return 0;
        }

        // Tracking -total to +total (+ 1 for the zero).
        int[][] dp = new int[nums.length][2 * total + 1];
        dp[0][total + nums[0]]++;
        dp[0][total - nums[0]]++;

        for (int i = 1; i < nums.length; i++) {
            for (int sum = -total; sum <= total; sum++) {
                int val = dp[i - 1][sum + total];
                if (val > 0) {
                    dp[i][sum + total + nums[i]] += val;
                    dp[i][sum + total - nums[i]] += val;
                }
            }
        }
        return dp[dp.length - 1][total + target];
    }
}
```

### Prior counts

Motivated by the prior solution how about if we just keep the counts for the
prior reachable values, and for each new element create the new counts? We can
do this with a two hash map approach.

The time complexity for this approach is O(t * n) where t is the total of
`nums` and `n` is the number of elements. The space complexity is O(2^n). This
comes from each iteration potential doubling the number of elements in the
counting map.

```java
class Solution {
    public int findTargetSumWays(int[] nums, int target) {
        Map<Integer, Integer> curr = new HashMap<>();
        Map<Integer, Integer> next = new HashMap<>();
        curr.put(nums[0], 1);
        curr.put(-nums[0], curr.getOrDefault(-nums[0], 0) + 1);
        for (int i = 1; i < nums.length; i++) {
            int val = nums[i];
            next.clear();
            for (Map.Entry<Integer, Integer> entry : curr.entrySet()) {
                int plusVal = entry.getKey() + val;
                int oldCount = entry.getValue();
                next.put(plusVal, next.getOrDefault(plusVal, 0) + oldCount);
                int minusVal = entry.getKey() - val;
                next.put(minusVal, next.getOrDefault(minusVal, 0) + oldCount);
            }
            curr = new HashMap<>(next);
        }

        return curr.getOrDefault(target, 0);
    }
}
```
