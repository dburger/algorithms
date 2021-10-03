# Find Unique Binary String

Given an array of strings `nums` containing `n` unique binary strings each of
length `n`, return a binary string of length `n` that does not appear in
`nums`. If there are multiple answers you may return any of them.

## Hints

1. Think math, think Cantor.

## Solutions

### Cantor could do it

This problem is very easy if you are familar with
[Cantor's diagonal argument](https://en.wikipedia.org/wiki/Cantor%27s_diagonal_argument).
Cantor's argument explains how to generate a unique binary sequence by going
through the binary strings and choosing the complementing bit for the first
bit in string one for the first bit, the complementing bit for the second
bit in string two, and so on. With this approach it is clear that the
generated string will be different than each input string.

The time complexity for this solution is O(n) where n is the length of the
input strings. The space complexity is also O(n), in order to hold the result
string.

I wouldn't call this a medium problem. This problem is a pretty good
identifier for how much math exposure one has.

```java
class Solution {
    public String findDifferentBinaryString(String[] nums) {
        StringBuilder buf = new StringBuilder();
        for (int i = 0; i < nums.length; i++) {
            char c = nums[i].charAt(i) == '0' ? '1' : '0';
            buf.append(c);
        }
        return buf.toString();
    }
}
```
