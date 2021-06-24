# Interleaving Strings

Given strings `s1`, `s2`, and `s3`, find whether `s3` is formed by an
interleaving of `s1` and `s2`.

An **interleaving** of two strings `s` and `t` is a configuration where they
are divided into **non-empty** substrings such that:

*   `s = s1 + s2 + ... + sn`
*   `t = t1 + t2 + ... + tn`
*   The **interleaving** is `s1 + t1 + s2 + t2 + s3 + t3...`
    or `t1 + s1 + t2 + s2 + t3 + s3...`

## Hints

1. Try thinking in terms of consuming `s1` and `s2` as you can while recursing
   on the remainder. This should give you a brute force approach.
1. There is dynamic programming goodness here. Both memoization and table
   approaches are doable.

## Solutions

This is a great problem in terms of clearly showing classic dynamic programming
problem solving approaches. The brute force approach above can be made viable
by bolting on some memoization. A table dynamic programming approach also works
well with this problem.

### Brute force

As noted in the hints a brute force approach can be constructed that works in
terms of "consuming" `s1` and `s2`. This approach will fail the leeter's time
limits but is shown here as it provides the basis for the following dynamic
programming solutions.

The space complexity of this solution is O(m + n) where m is the length of
`s1` and n is the length of `s2`. This follows from the depth of the recursion,
where a character of `s1` or `s2` is consumed on each recursive call.

```java
class Solution {
    public boolean isInterleave(String s1, String s2, String s3) {
        if (s1.length() + s2.length() != s3.length()) {
            return false;
        }
        return isInterleave(s1, 0, s2, 0, s3, 0);
    }

    private static boolean isInterleave(String s1, int s1idx, String s2, int s2idx, String s3, int s3idx) {
        if (s1idx == s1.length() && s2idx == s2.length()) {
            // We have fully consumed s1 and s2
            return true;
        }
        boolean result = false;
        if (s1idx < s1.length() && s1.charAt(s1idx) == s3.charAt(s3idx)) {
            // consume a match from s1
            result = isInterleave(s1, s1idx + 1, s2, s2idx, s3, s3idx + 1);
        }
        if (!result && s2idx < s2.length() && s2.charAt(s2idx) == s3.charAt(s3idx)) {
            // consume a match from s2
            result = isInterleave(s1, s1idx, s2, s2idx + 1, s3, s3idx + 1);
        }
        return result;
    }
}
```

### Memoize me

The above solution fails the leeter's time limits. The problem is that it
recomputes the same subproblems repeatedly. For example, say that some
recursion of the problem arrived at a point where n characters have been
consumed from `s1` and p characters have been consumed from `s2`. If a
later recursive invocatoin comes along that arrived at the same n
characters from `s1` and p characters from `s2` already consumed, but in
a different order, it will continue from that subproblem point attempting
a match. Of course this is of no use, as the subproblem that remains has
already been computed and is going to result in failure. The classic
dynamic programming approach to fixing this problem is to throw in a
memoizer to prevent repeated subproblem computation.

The time and space complexity for this solution is O(m * n) where m is the
length of `s1` and n is the length of `s2`. This is because the number of
computations and the size of the memo is bound by having to potentially
compute each `(m, n)` position once.

```java
class Solution {
    public boolean isInterleave(String s1, String s2, String s3) {
        if (s1.length() + s2.length() != s3.length()) {
            return false;
        }
        Map<String, Boolean> memo = new HashMap<>();
        return isInterleave(s1, 0, s2, 0, s3, 0, memo);
    }

    private static boolean isInterleave(String s1, int s1idx, String s2, int s2idx, String s3, int s3idx, Map<String, Boolean> memo) {
        String key = s1idx + ":" + s2idx;
        if (memo.containsKey(key)) {
            return memo.get(key);
        }
        if (s1idx == s1.length() && s2idx == s2.length()) {
            // We have fully consumed s1 and s2
            return true;
        }
        boolean result = false;
        if (s1idx < s1.length() && s1.charAt(s1idx) == s3.charAt(s3idx)) {
            // consume a match from s1
            result = isInterleave(s1, s1idx + 1, s2, s2idx, s3, s3idx + 1, memo);
        }
        if (!result && s2idx < s2.length() && s2.charAt(s2idx) == s3.charAt(s3idx)) {
            // consume a match from s2
            result = isInterleave(s1, s1idx, s2, s2idx + 1, s3, s3idx + 1, memo);
        }
        memo.put(key, result);
        return result;
    }
}
```

### Better memoizer

Note the use of the key `String key = s1idx + ":" + s2idx;` in the prior
solution. This works well and is straightforward. It does, however, have a
downside in terms of efficiency. All of the string instantations result in
a performance hit in practice. The following solution gets rid of this hit
by using an int memo table. The table at value `(i, j)` holds the memoized
result for the subproblem starting at positions `i` and `j` of `s1` and `s2`
respectively. A `0` indicates the subproblem has not been computed yet. A
`1` indicates a solution can be found from that starting point. A `-1`
indicates a solution cannot be found from that starting point.

This solution aces the leeter's time limit grader. It even beats the table
approach below.

The space and time complexity of this solution is also O(m * n) as each
position in the `memo` array is potentially computed once.

```java
class Solution {
    public boolean isInterleave(String s1, String s2, String s3) {
        if (s1.length() + s2.length() != s3.length()) {
            return false;
        }
        int[][] memo = new int[s1.length() + 1][s2.length() + 1];
        return isInterleave(s1, 0, s2, 0, s3, 0, memo);
    }

    private static boolean isInterleave(String s1, int s1idx, String s2, int s2idx, String s3, int s3idx, int[][] memo) {
        int mval = memo[s1idx][s2idx];
        if (mval != 0) {
            return mval == 1;
        }
        if (s1idx == s1.length() && s2idx == s2.length()) {
            // We have fully consumed s1 and s2
            return true;
        }
        boolean result = false;
        if (s1idx < s1.length() && s1.charAt(s1idx) == s3.charAt(s3idx)) {
            // consume a match from s1
            result = isInterleave(s1, s1idx + 1, s2, s2idx, s3, s3idx + 1, memo);
        }
        if (!result && s2idx < s2.length() && s2.charAt(s2idx) == s3.charAt(s3idx)) {
            // consume a match from s2
            result = isInterleave(s1, s1idx, s2, s2idx + 1, s3, s3idx + 1, memo);
        }
        memo[s1idx][s2idx] = result ? 1 : -1;
        return result;
    }
}
```

### Dynamic programming table approach

Here we use a dynamic programming table approach where entry `(i, j)` is set to
true if the first `i` characters from `s1` and the first `j` characters from
`s2` is a solution. It is seeded by doing position `(0, 0)` which is always
true and then the first column and row which are easily determined by looking
at the prior position and the new character. The body is then filled out in
a similar manner where it is necessary to consider either consuming the next
character in `s1` or the next character in `s2`.

The time and space complexity of this solution is O(n * m) where n is the length
of `s1` and m is the length of `s2`. This follows from the size of the table.

```java
class Solution {
    public boolean isInterleave(String s1, String s2, String s3) {
        if (s1.length() + s2.length() != s3.length()) {
            return false;
        }
        boolean[][] dp = new boolean[s1.length() + 1][s2.length() + 1];
        // 0 characters from s1 and 0 characters from s2 matches 0 characters from s3
        dp[0][0] = true;
        // Set first column, prefix of s1 that matches s3.
        for (int i = 1; i <= s1.length(); i++) {
            dp[i][0] = dp[i - 1][0] && s1.charAt(i - 1) == s3.charAt(i - 1);
        }
        // Set first row, prefix of s2 that matches s3.
        for (int i = 1; i <= s2.length(); i++) {
            dp[0][i] = dp[0][i - 1] && s2.charAt(i - 1) == s3.charAt(i - 1);
        }
        // Now the body. Position (i, j) works if either:
        //   (i - 1, j) worked and the next s1 character matches the s3 character.
        //   (i, j - 1) worked and the next s2 character matches the s3 character.
        for (int i = 1; i <= s1.length(); i++) {
            for (int j = 1; j <= s2.length(); j++) {
                dp[i][j] = (dp[i - 1][j] && s1.charAt(i - 1) == s3.charAt(i + j - 1)) || (dp[i][j - 1] && s2.charAt(j - 1) == s3.charAt(i + j - 1));
            }
        }
        return dp[s1.length()][s2.length()];
    }
}
```
