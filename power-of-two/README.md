# Power of Two

Given an integer `n`, return `true` if it is a power of two. Otherwise return
false.

An integer `n` is a power of two, if there exists an integer `x` such that
`n == 2^x`.

## Hints

1. Dividing down to zero would work, but is it efficient?

## Solutions

### Count the bits

There are likely many ways to solve this. One way would be to repeatedly take
the value modulus `2`, make sure that was `0`, and then divide by `2` to reduce the number. This would continue until we get down to `1` or fail to have an
even number. Quite easy and time complexity O(log(n)). We can do better,
however, by looking at the bits. An `int` is a power of two only if it is
positive and has `1` bit set.

This solution has O(1) time and space complexity.

```java
class Solution {
    public boolean isPowerOfTwo(int n) {
        return n > 0 && Integer.bitCount(n) == 1;
    }
}
```

### Same as last, but now over optimized

The last solution is great, however, the leetcode grader wasn't too impressed
by its speed. If we replace the call to `Integer.bitCount` with a hand
rolled inline call we make the grader as happy as can be. This version exploits
the fact that taking `n & (n - 1)` removes the lowest bit. If that took the
bit count to zero, we have a power of two.

```java
class Solution {
    public boolean isPowerOfTwo(int n) {
        return n > 0 && (n & (n - 1)) == 0;
    }
}
```
