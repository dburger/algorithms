# Rotate Function

You are given an integer array `nums` of length `n`.

Assume `arr_k` to be an array obtained by rotating `nums` by `k` positions
clock-wise. We define the **rotation function** `F` on `nums as follow:

- `F(k) = 0 * arr_k[0] + 1 * arr_k[1] + ... + (n - 1) * arr_k[n - 1]`

Return the maximum value of `F(0), F(1), ..., F(n - 1)`.

The test cases are generated so that the answer fits in a **32-bit** integer.

## Hints

1. Can you determine F(n + 1) from F(n)?

## Solutions

This is a great problem because it has a very easy brute force solution and
a dynamic programming solution. If this problem came up in an interview you
would likely want to describe the brute force solution and then jump to
implementing with the dynamic programming approach.

### Brute it

As mentioned above the brute force solution for this is not difficult. This
solution computes the value for each of the rotations and tracks the max.

The time complexity of this solution is O(n^2) where `n` is the length of
the input `nums`. The space complexity of this solution is O(1). It uses
the same space regardless of the size of the input.

```java
class Solution {
    public int maxRotateFunction(int[] nums) {
        int max = Integer.MIN_VALUE;
        for (int i = 0; i < nums.length; i++) {
            int sum = 0;
            for (int j = 0; j < nums.length; j++) {
                sum += j * nums[(j + i) % nums.length];
            }
            if (sum > max) {
                max = sum;
            }
        }
        return max;
    }
}
```

### What is your recurrence?

As noted above there is a dynamic programming approach to solving this problem.
If you can determine the recurrence relation between F(n) and F(n + 1) this
dynamic programming approach easily follows.

To think to the approach, consider a trivial example. Say you have the array
with values `[a, b, c, d]`. Then `F(0)` and `F(1)` are:

`F(0) == 0*a + 1*b + 2*c + 3*d`
`F(1) == 0*d + 1*a + 2*b + 3*c`

The recurrence can be seen. Say that `F(0) == x`. Then in the step to `F(1)` we
have the following changes to `x`.

- The last term from the prior sum is removed
- Each of the other terms from the prior sum are increased by a multiple of 1

This correponds to the following:

`F(n + 1) = F(n) - (nums.length - 1)(L) + (allSum - L)`

Where `L` is the last term from the prior sum and `allSum` is the sum of all
the values in `nums`.

So, this problem can be solved by starting with determining `allsum` and the
sum for `F(0)`. The finish involves using the above recurrence relation to
solve for `F(1), ..., F(n - 1)`. The time complexity of this solution is
O(n) where `n` is the length of `nums`. This comes from the loops which iterate
`n` times. The space complexity of this solution is O(1).

```java
class Solution {
    public int maxRotateFunction(int[] nums) {
        int oneSum = 0;
        int allSum = 0;
        for (int i = 0; i < nums.length; i++) {
            int val = nums[i];
            oneSum += i * val;
            allSum += val;
        }

        int max = oneSum;

        for (int i = 1; i < nums.length; i++) {
            // The prior last.
            int last = nums[nums.length - i];
            // oneSum = oneSum - (nums.length - 1) * last + allSum - last;
            oneSum += allSum - nums.length * last;
            if (oneSum > max) {
                max = oneSum;
            }
        }

        return max;
    }
}
```
