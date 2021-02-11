# Longest Palindromic Substring

Given a string `s`, return the longest palindromic substring in `s`.

## Hints

1. An obvious brute approach would just go through each starting point and
   attempt to "expand" from there.
1. Classically set up for a dynamic programming approach. Think of how to
   build up the table.

## Solutions

### Obvious brute

This approach follows the first hint and merely attempts to expand from each
starting point.

Note that this approach is not as "brutish" as it appears! It actually beats
the dynamic programming approach below.

This approach as time complexity O(n^2) as it takes `n` iterations to get
through the string and each attempt at expansion can expand up to `n / 2`
times. The space complexity is O(1).

```java
class Solution {
    public String longestPalindrome(String s) {
        int n = s.length();
        int max = 1;
        int maxStart = 0;
        int maxEnd = 0;

        for (int i = 0; i < n - 1; i++) {
            // Starting from one character palindromes.
            int start = i;
            int end = i;
            int j = 1;
            for (; start - j > -1 && end + j < n; j++) {
                if (s.charAt(start - j) != s.charAt(end + j)) {
                    break;
                }
            }
            j--;
            // int len = end + j - (start - j) + 1;
            int len = end - start + 2 * j + 1;
            if (len > max) {
                max = len;
                maxStart = start - j;
                maxEnd = start + j;
            }

            // Now try starting from two character palindromes.
            if (s.charAt(i) == s.charAt(i + 1)) {
                start = i;
                end = i + 1;
                j = 1;
                for (; start - j > -1 && end + j < n; j++) {
                    if (s.charAt(start - j) != s.charAt(end + j)) {
                        break;
                    }
                }
                j--;
                len = end - start + 2 * j + 1;
                if (len > max) {
                    max = len;
                    maxStart = start - j;
                    maxEnd = end + j;
                }
            }
        }
        return s.substring(maxStart, maxEnd + 1);
    }
}
```

### Obvious brute but extract out `expand`

This is the same solution as above with the `expand` function factored out for
use in both the starting from a single character and starting from two character
cases.

```java
class Solution {
    public String longestPalindrome(String s) {
        int n = s.length();
        int max = 1;
        int maxStart = 0;
        int maxEnd = 0;

        for (int i = 0; i < n - 1; i++) {
            // Starting from one character palindromes.
            int start = i;
            int end = i;
            int hops = expand(s, start, end);
            // int len = end + hops - (start - hops) + 1;
            // where end and start are the same
            int len = 2 * hops + 1;
            if (len > max) {
                max = len;
                maxStart = start - hops;
                maxEnd = end + hops;
            }

            // Now try starting from two character palindromes.
            start = i;
            end = i + 1;
            hops = expand(s, start, end);
            len = end - start + 2 * hops + 1;
            if (len > max) {
                max = len;
                maxStart = start - hops;
                maxEnd = end + hops;
            }
       }
        return s.substring(maxStart, maxEnd + 1);
    }

    /**
     * Returns the number of "hops" that can be taken away from {@code start}
     * and {@code end} and still have a palindrome. Note that {@code -1} is
     * returned if {@code start} and {@code end} do not represent a palindrome
     * to begin with (only for 2 character start obviously).
     */
    private int expand(String s, int start, int end) {
        int n = s.length();
        int j = 0;
        for (; start - j > - 1 && end + j < n; j++) {
            if (s.charAt(start - j) != s.charAt(end + j)) {
                break;
            }
        }
        return --j;
    }
}
```

### Dynamic programming

This approach is a classic dynamic approach where you build up a table where
the entry at `(i, j)` is true iff the string hosts a palindrome at the substring
`[i, j]`. This problem contains an interesting wrinkle. In many dynamic
programming problems you need to initialize one aspect and then build up the
remaining solutions from that starting point. In this dynamic programming
problem there are actually two related aspects to initialize. That is, one
character palindromes and possible two character palindromes must be initialized
before building solutions on those foundations.

Note the important point here that anything below the left to right diagonal
need not be computed as those are entries with the ending index before the
starting index, and thus are invalid.

The time complexity for this algorithm in O(n^2) where `n` is the length of the
string. This has to do with filling out half of the `n x n` table. The space
complexity is also O(n^2) for the same reason.

```java
class Solution {
    public String longestPalindrome(String s) {
        int n = s.length();
        boolean[][] dp = new boolean[n][n];

        // All single characters represent a trivial
        // palindromic substring.
        int max = 1;
        int maxStart = 0;
        int maxEnd = 0;
        for (int i = 0; i < n; i++) {
            dp[i][i] = true;
        }

        // Some of the 2 character substrings may be
        // a palindromic substring.
        for (int row = 0; row < n - 1; row++) {
            int col = row + 1;
            if (s.charAt(row) == s.charAt(col)) {
                dp[row][col] = true;
                if (max < 2) {
                    max = 2;
                    maxStart = row;
                    maxEnd = col;
                }
            }
        }

        // Now the remaining upper half of the table can be filled in.
        // Notice how we start in the bottom right and work our way
        // upwards and to the left to work off already known entries.
        for (int row = n - 3; row > -1; row--) {
            for (int col = n - 1; col > row + 1; col--) {
                if (dp[row + 1][col - 1] && s.charAt(row) == s.charAt(col)) {
                    dp[row][col] = true;
                    int len = col - row + 1;
                    if (len > max) {
                        max = len;
                        maxStart = row;
                        maxEnd = col;
                    }
                }
            }
        }

        return s.substring(maxStart, maxEnd + 1);
    }
}
```
