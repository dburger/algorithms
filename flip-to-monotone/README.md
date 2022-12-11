# Flip String to Monotone Increasing

A binary string is monotone increasing if it consists of some number of `0`'s
(possibly none), followed by some number of `1`'s (also possibly none).

You are given a binary string `s`. You can flip `s[i]` changing it from `0` to
`1` or from `1` to `0`.

Return the minimum number of flips to make `s` monotone increasing.

## Hints

1. Think in terms of halves.

## Solutions

### What's your prefix?

This problem can be solved with a prefix array approach and thinking in terms
of halves. The prefix array is constructed to count the number of ones before
position `i`. Then a second pass can be used where the count to form a monotonic
string is the number of ones before plus the number of zeroes after each
position. The minimum of this count is the minimum number of flips to form
the string. This solution is rather easy to understand, however, there are a
ton of "off by one" possibilites and using the wrong `s` or `prefix` length
possible here.

The time complexity of this solution is O(n) where `n` is the lenth of the input
string. This comes from the two passes of size `n`. The space complexity is also
O(n), which comes from the construction of the `prefix` array.

```java
class Solution {
    public int minFlipsMonoIncr(String s) {
        int[] prefix = new int[s.length() + 1];
        for (int i = 0; i < s.length(); i++) {
            prefix[i + 1] = prefix[i] + (s.charAt(i) == '1' ? 1 : 0);
        }
        int min = Integer.MAX_VALUE;
        for (int i = 0; i < prefix.length; i++) {
            int onesBefore = prefix[i];
            // zeroesAfter = length remaining - ones after
            // int zeroesAfter = s.length() - i - (prefix[prefix.length - 1] - prefix[i]);
            int zeroesAfter = s.length() - i - prefix[prefix.length - 1] + prefix[i];
            int flips = onesBefore + zeroesAfter;
            if (flips < min) {
                min = flips;
            }
        }
        return min;
    }
}
```
