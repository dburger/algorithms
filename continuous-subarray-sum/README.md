# Continuous Subarray Sum

Given an integer array `nums` and an integer `k`, return `true` if `nums` has
a continuous subarray of size **at least two** whose elements sum up to a
multiple of `k`, or `false` otherwise.

An integer `x` is a multiple of `k` if there exists an integer `n` such that
`x = n * k`. `0` is **always** a multiple of `k`.

## Hints

1. Brute force could do this easily, but will it pass the grader?
1. Think in terms of the prefix sum.

## Solutions

### Brute, but no cigar for you

A brute force solution can create every sum and check if it is divisible by
`k`. We increment through rounds where we create all the sums that start at
position `0`, then all at position `1`, and so on.

The time complexity of this solution is O(n^2) where n is the number of
elements in `nums`. The space complexity of this solution is O(1). No, it will
not pass the leet grader. It times out.

```java
class Solution {
    public boolean checkSubarraySum(int[] nums, int k) {
        for (int i = 0; i < nums.length; i++) {
            int sum = nums[i];
            for (int j = i + 1; j < nums.length; j++) {
                sum += nums[j];
                if (sum % k == 0) {
                    return true;
                }
            }
        }
        return false;
    }
}
```

### Let 'er hum, with a Prefix Sum

So the O(n^2) solution times out. How can we speed this up? We can leverage
modular arithmetic. We do this by keeping track of the modular sum, and a set
of previously seen sums. When we compute the new modular sum we check if this
prefix had already been seen. If it has, then we could chop off that portion
of the prefix sum and get back to `0`. Notice that we don't add the remainder
until it has become the "one prior" sum. This enforces the condition that the
solution must have more than 1 element.

The time complexity of this solution is O(n) where n is the length of `nums`.
This comes from a potential full pass through the array computing prefix sums
and checking for a prior occurence. The space complexity of this solution is
O(k). This comes from the possible need to hold up to `k` elements in the
checking set.

```java
class Solution {
    public boolean checkSubarraySum(int[] nums, int k) {
        Set<Integer> seen = new HashSet<>();
        int prev = 0;
        int rem = 0;
        for (int i = 0; i < nums.length; i++) {
            rem = (prev + nums[i]) % k;
            if (seen.contains(rem)) {
                return true;
            }
            seen.add(prev);
            prev = rem;
        }
        return false;
    }
}
```
