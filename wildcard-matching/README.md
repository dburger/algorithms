# Wildcard Matching

Given an input string `s` and a pattern `p`, implement wildcard pattern matching
with support for `?` and `*` where:

* `?` Matches any single character.
* `*` Matches any sequence of characters (including the empty sequence)

The matching should cover the **entire** string (not partial).

## Hints

1. A dynamic programming solution builds the solution in a table. The table
   can hold true / false for a match of the string up to position `i` and
   pattern up to position `j`.

## Solutions

### Dynamic programming

The dynamic programming solution builds up a table as indicated in the hint. Since
this approach visits every cell in the table, it is O(len(s) * len(p)). Be careful
with indices, the table itself is one based with zero representing empty. Thus the
length plus one in loops and index minus one in character access.

```java
class Solution {
    public boolean isMatch(String s, String p) {
        // table[i][j] true if string up to character i and pattern up to p
        // matches
        boolean[][] table = new boolean[s.length() + 1][p.length() + 1];

        // Empty pattern matches empty string.
        table[0][0] = true;

        // Empty string only matches pattern if all *.
        for (int j = 1; j < p.length() + 1; j++) {
            table[0][j] = table[0][j - 1] && p.charAt(j - 1) == '*';
        }

        // Empty pattern never matches non-empty string, since false by default,
        // this loop is unnecessary.
        /*
        for (int i = 1; i < s.length(); i++) {
            table[i][0] = false;
        }
        */

        for (int i = 1; i < s.length() + 1; i++) {
            for (int j = 1; j < p.length() + 1; j++) {
                switch (p.charAt(j - 1)) {
                    case '*':
                        // If matched without the pattern character or consuming it.
                        table[i][j] = table[i][j - 1] || table[i - 1][j];
                        break;
                    case '?':
                        // If matched on prior character and pattern element.
                        table[i][j] = table[i - 1][j - 1];
                        break;
                    default:
                        // If matched on prior character and pattern element, and additional
                        // character and pattern element.
                        table[i][j] = table[i - 1][j - 1] && s.charAt(i - 1) == p.charAt(j - 1);
                        break;
                }
            }
        }

        return table[s.length()][p.length()];
    }
}
```
