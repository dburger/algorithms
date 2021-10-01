# Longest Common Subsequence

Given two strings `text1` and `text2`, return the length of their longest
common subsequence. If there is no common subsequence return `0`.

A subsequence of a string is a new string generated from the original string
with some characters (can be none) deleted without changing the relative order
of the remaining characters. For example `"ace"` is a subsequence of `"abcde"`.

A common subsequence of two strings is a subsequence that is common to both
strings.

## Hints

1. Classic dynamic programming.

## Solutions

### Builder yer dp table

This solution follows a classic dynamic programming approach. Here we compute
a solution by building up a table where position `(i, j)` is the longest
common subsequence for `text1` up to character `i` and `text2` up to character
`j`. The values in the first row and first column are all `0`, as the longest
common subsequence between anything and an empty string is the empty string.
For all the interior positions, the value can be calculated as follows:

*   If the characters match, the value is `dp[i - i][j - 1] + 1`
*   If the characters don't match, the value is the max of `dp[i - 1][j]`
    and `dp[i][j - 1]`

In the second case, this means we are carrying the max value forward with the
inclusion of one of the new characters as we take the position that maximizes
the inclusion of the other.

The time complexity for this solution is O(m * n) where m is the length of
`text1` and n is the length of `text2`. This comes from needing to fill out
the table and the iteration that visits each cell. Likewise, the space
complexity is also O(m * n) to hold that same table in memory.

```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        int[][] dp = new int[text1.length() + 1][text2.length() + 1];
        for (int i = 1; i <= text1.length(); i++) {
            for (int j = 1; j <= text2.length(); j++) {
                if (text1.charAt(i - 1) == text2.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }
        return dp[text1.length()][text2.length()];
    }
}
```
