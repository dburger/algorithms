# Regular Expression Matching

Given an input string `s` and pattern `p`, implement regular expression matching
support for `.` and `*` where:

* `.` Matches any single character.
* `*` Matches zero of more of the preceding character.

The matching should cover the `entire` input string (not partial).

## Hints

1. Think in terms of "consuming" the string and pattern until they are empty.

## Solutions

### Consume to completion

Check for first character match, and then branch on whether that character is
followed by a `*` or not. If it is, try consuming a single character in the
string or ditching the `*` for no more matches of that character. If it is not
advance both the string and the pattern.

```java
class Solution {
    public boolean isMatch(String s, String p) {
        if (p.isEmpty()) {
            return s.isEmpty();
        }

        boolean firstMatch = !s.isEmpty() && (s.charAt(0) == p.charAt(0) || p.charAt(0) == '.');

        if (p.length() > 1 && p.charAt(1) == '*') {
            return isMatch(s, p.substring(2)) || (firstMatch && isMatch(s.substring(1), p));
        } else {
            return firstMatch && isMatch(s.substring(1), p.substring(1));
        }
    }
}
```

### Dynamic programming

Same as above, but using a memo to avoid repeated computations. This makes the
much faster as a dynamic programming solution.

```java
class Solution {
    public boolean isMatch(String s, String p) {
        Boolean[][] memo = new Boolean[s.length() + 1][p.length() + 1];
        return isMatch(0, 0, s, p, memo);
    }

    public boolean isMatch(int i, int j, String s, String p, Boolean[][] memo) {
        if (memo[i][j] != null) {
            return memo[i][j];
        }

        if (j == p.length()) {
            return i == s.length();
        }

        boolean firstMatch = i < s.length() && (s.charAt(i) == p.charAt(j) || p.charAt(j) == '.');

        if (j < p.length() - 1 && p.charAt(j + 1) == '*') {
            boolean result = isMatch(i, j + 2, s, p, memo) || firstMatch && isMatch(i + 1, j, s, p, memo);
            memo[i][j] = result;
            return result;
        } else {
            boolean result = firstMatch && isMatch(i + 1, j + 1, s, p, memo);
            memo[i][j] = result;
            return result;
        }
    }
}
```
