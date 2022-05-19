# Greatest Common Divistor of Strings

For two strings `s` and `t`, we say "`t` divides `s`" if and only if
`s = t + ... + t` (i.e., `t` is concatenated with itself one or more times).

Given two `str1` and `str2`, return the largest string `x` such that `x`
divides both `str1` and `str2`.

## Hints

1. Brute force could walk out to the longest common prefix and then walk back
   until finding the first one that works.
1. Can you determine if there is a gcd with simple check?

## Solutions

### Oh Euclid is great

This solution uses an approach that might be difficult to come up with in an
interview situation. It works by first checking if the gcd exists or not by
checking if the strings concatenated in either order produce the same string.
If they do, then there is a gcd. This is followed by determining the gcd. The
gcd segment will be equal to the gcd of the lengths of the two strings.

The time and space complexity of this algorithm is O(log(min(m, n))) where
`m` is the length of `str1` and `n` is the length of `str2`. This comes from
the time complexity of Euclid's algorithm for determining a GCD. In each
iteration the smaller value is halved until reaching zero, thus the log
complexity.

```java
class Solution {
    public String gcdOfStrings(String str1, String str2) {
        if (!(str1 + str2).equals(str2 + str1)) {
            return "";
        }

        int len = gcd(str1.length(), str2.length());
        return str1.substring(0, len);
    }

    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
}
```
