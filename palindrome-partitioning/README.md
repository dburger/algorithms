# Palindrome Partitioning

Given a string `s`, partition `s` such that every substring of the partition is
a **palindrome**. Return all possible plaindrome partitionings of `s`.

## Hints

1. Find a palindrome at the front of a string and recurse on the tail until
   exhausted?

## Solutions

### Find and recurse

This is a common pattern for solving these types of problems that work by
exhausting an input by recursing on it and storing results in an accumulator.

```java
class Solution {
    public List<List<String>> partition(String s) {
        List<List<String>> acc = new ArrayList<>();
        partition(s, 0, new ArrayList<>(), acc);
        return acc;
    }

    private void partition(String s, int start, List<String> parts, List<List<String>> acc) {
        if (start == s.length()) {
            acc.add(new ArrayList<>(parts));
            return;
        }
        for (int i = start + 1; i <= s.length(); i++) {
            String part = s.substring(start, i);
            if (isPalindrome(s, start, i)) {
                parts.add(part);
                partition(s, i, parts, acc);
                parts.remove(parts.size() - 1);
            }
        }
    }

    private boolean isPalindrome(String s, int start, int end) {
        while (start < end) {
            if (s.charAt(start++) != s.charAt(--end)) {
                return false;
            }
        }
        return true;
    }
}
```

### Dynamic programming

This solution builds up a table that indicates where the palindromes are within
the string. Then it uses that table in a recursive invocation. The recursion
loops over the indicated row attempting to find a palindrome. When it finds one
that goes from `dp[row][col]`, then it tries to continue that by finding the
palindromes that start in `row == col + 1`. (Since the previous match ended at
position `col`). If it can do that through the bottom row then a full
partitioning has been found.

I thought this solution would be much faster than the one above, but alas
leeters says it is about the same to only slightly faster. Note that to even get
this speed boost special attention was paid to only process the valid part of
the dynamic programming table - that is cells where `(row, col)` makes sense.
That is `col >= row`.

```java
class Solution {
    public List<List<String>> partition(String s) {
        int n = s.length();
        boolean[][] dp = new boolean[n][n];
        // All individual characters are palindromes.
        for (int i = 0; i < n; i++) {
            dp[i][i] = true;
        }
        // Mark the 2 character palindromes.
        for (int i = 0; i < n - 1; i++) {
            dp[i][i + 1] = s.charAt(i) == s.charAt(i + 1);
        }
        // Now loop over the remaining cells marking them as palindromes.
        // Note that we only iterate valid (start, end) index combinations.
        for (int r = n - 3; r >= 0; r--) {
            for (int c = r + 2; c < n; c++) {
                dp[r][c] = dp[r + 1][c - 1] && s.charAt(r) == s.charAt(c);
            }
        }
        List<List<String>> acc = new ArrayList<>();
        partition(s, dp, 0, new ArrayList<>(), acc);
        return acc;
    }

    private void partition(String s, boolean[][] dp, int row, List<String> parts, List<List<String>> acc) {
        if (row == dp.length) {
            acc.add(new ArrayList<>(parts));
            return;
        }
        for (int col = row; col < dp[0].length; col++) {
                if (dp[row][col]) {
                    parts.add(s.substring(row, col + 1));
                    partition(s, dp, col + 1, parts, acc);
                    parts.remove(parts.size() - 1);
                }
        }
    }
}
```
