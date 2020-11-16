# Edit Distance

Given two strings `word1` and `word2`, return the minimum number of operations
required to convert `word1` to `word2`.

You have the following three operations permitted on a word:

* Insert a character
* Delete a character
* Replace a character

## Hints

1. This is another one of those classic dynamic programming example problems.
   This of building up a table and iterating over it where the table tracks
   the number of operations of using m characters of `word1` and n characters
   of `word2`.

## Solutions

### Classic dynamic programming

This solution proceeds by building a table where `(m, n)` represents the
minimum edit distance taking `m` characters of `word1` to make `n` characters
of `word2`. The first column and row can be filled out trivially. The body
either gets a match or the minimum amongst insert, delete, or replace.

The solution is O(m*n).

```java
class Solution {
    public int minDistance(String word1, String word2) {
        int m = word1.length();
        int n = word2.length();
        if (m == 0) {
            // all insert
            return n;
        }
        if (n == 0) {
            // all delete
            return m;
        }
        int dp[][] = new int[m + 1][n + 1];
        // first column, zero characters from word2, all delete of word1
        for (int i = 0; i < m + 1; i++) {
            dp[i][0] = i;
        }
        // first row, zero characters from word1, all insert of word2
        for (int j = 0; j < n + 1; j++) {
            dp[0][j] = j;
        }
        // table body, either match or minimum among three edits
        for (int i = 1; i < m + 1; i++) {
            for (int j = 1; j < n + 1; j++) {
                if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                    // characters match, no edit needed
                    dp[i][j] = dp[i - 1][j - 1];
                } else {
                    // characters don't match, use minimum of edits
                    int insert = dp[i - 1][j];
                    int delete = dp[i][j - 1];
                    int replace = dp[i - 1][j - 1];
                    int min = insert < delete ?
                        (insert < replace ? insert : replace) :
                        (delete < replace ? delete : replace);
                    dp[i][j] = 1 + min;
                }
            }
        }
        return dp[m][n];
    }
}
```
