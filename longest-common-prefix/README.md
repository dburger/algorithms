# Longest Common Prefix

Write a function to find the longest common prefix string amongst an array of
strings. If there is no common prefix, return an empty string "".

## Hints

1. Nothing complicated here, just march along from position zero and advance
   till failure.

## Solutions

### Advance to failure

This solution starts a loop that iterates through the characters of the first
string. It stops under two conditions:

*   It finds another string that is shorter than the position it is looking at.
*   It finds another string with a character that does not match at that
    position.

The time complexity of this algirthm is O(n * m) where n is the number of
strings and m is the length of the longest common prefix. When all strings
match start to finish this is O(n^2).

```java
class Solution {
    public String longestCommonPrefix(String[] strs) {
        int len = strs.length;
        if (len == 0) {
            return "";
        } else if (len == 1) {
            return strs[0];
        } else {
            String first = strs[0];
            int flen = first.length();
            for (int i = 0; i < flen; i++) {
                char c = first.charAt(i);
                for (int j = 1; j < len; j++) {
                    String candidate = strs[j];
                    if (i >= candidate.length() || candidate.charAt(i) != c) {
                        return first.substring(0, i);
                    }
                }
            }
            return first;
        }
    }
}
```
