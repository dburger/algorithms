# Delete Operation for Two Strings

Given two strings `word1` and `word2`, return the minimumum number of steps
required to make `word1` and `word2` the same.

In one step, you can delete exactly one character in either string.

## Hints

1. This can be solved with classic dynamic programming approaches.

## Solutions

### DP, for life

This solution uses a classic dynamic programming approach. Here we keep a
table with `word1` represented down the rows and `word2` the columns. Each
entry `(i, j)` in the table represents the number of deletes required to make
`word1(i)` equal to `word2(j)` where `i` is the number of characters from the
start of `word1` and `j` is the number of characters from `word2`. At each
position `(i, j)` we check if the characters match. If they do match, then no
delete is required and we can carry the delete value from `(i - 1, j - 1)`
forward. If they do not match we need to perform a delete. The value carried
forward is the minimum of `(i - 1, j)` and `(i, j - 1)` plus `1` for the
additional delete.

The time complexity of this algorithm is O(m * n) where m is the length of
`word1` and n is the length of `word2`. This comes from the need to make a
pass filling out the table. Likewise, the space complexity is O(m * n). This
obviously comes from holding that same table.

Note the neat trick used on filling out the first row and first column.
Since these correspond to just deleting the other string to match an empty
string they are merely a sequence from `0` up to `n` where `n` is the
length of the other string. This is very simply achieved by
`dp[i][j] = i + j` when `i == 0 || j == 0`.

```java
class Solution {
    public int minDistance(String word1, String word2) {
        int[][] dp = new int[word1.length() + 1][word2.length() + 1];
        for (int i = 0; i <= word1.length(); i++) {
            for (int j = 0; j <= word2.length(); j++) {
                if (i == 0 || j == 0) {
                    dp[i][j] = i + j;
                } else if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1];
                } else {
                    dp[i][j] = Math.min(dp[i - 1][j], dp[i][j - 1]) + 1;
                }
            }
        }
        return dp[word1.length()][word2.length()];
    }
}
```
