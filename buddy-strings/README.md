# Buddy Strings

Given two strings `s` and `goal`, return `true` if you can swap two letters in
`s` so the result is equal to `goal`, otherwise, return `false`.

Swapping letters is defined as taking two indices `i` and `j` (0-indexed) such
that `i != j` and swapping the characters at `s[i]` and `s[j]`.

- For example, swapping indices `0` and `2` in `"abcd"` results in `"cbad"`.

## Hints

1. I see no elegance here, but I see cases.

## Solutions

### Three cases, small crime

This problem can be solved by breaking the possibility into three cases. They
are:

- `s` and `goal` are not the same length or are less than size `2`, reject
- `s` and `goal` are same length and equal, accept only if see the same
   character more than once, as this allows a swap of that character
- `s` and `goal` are the same length but not equal, walk the strings looking
  for the differenc and then see if a swap fixes it

The time complexity of this solution is O(n) where n is the length of the
strings. This is because we must walk the strings to compare them character
by character. The space complexity of this solution is also O(1). The `seen`
count tracking variable is sized 26 to identify lower case characters. There
is nothing that needs to be sized relative to the input.

```java
class Solution {
    public boolean buddyStrings(String s, String goal) {
        if (s.length() != goal.length() || s.length() < 2) {
            return false;
        }

        if (s.equals(goal)) {
            // If we see the same character more than once a swap of those
            // characters can be used.
            boolean[] seen = new boolean[26];
            for (int i = 0; i < s.length(); i++) {
                int pos = s.charAt(i) - 'a';
                if (seen[pos]) {
                    return true;
                }
                seen[pos] = true;
            }
            return false;
        } else {
            // They weren't the same so we know we have at least one difference.
            int i = 0;
            while (true) {
                if (s.charAt(i) != goal.charAt(i)) {
                    break;
                }
                i++;
            }
            // Now look for a second difference.
            int j = i + 1;
            while (j < s.length()) {
                if (s.charAt(j) != goal.charAt(j)) {
                    break;
                }
                j++;
            }
            if (j == s.length()) {
                // Second difference not found.
                return false;
            }
            // We have two differences, if a swap doesn't make them match up
            // this can't be fixed with swapping just two letters.
            if (s.charAt(i) != goal.charAt(j) || s.charAt(j) != goal.charAt(i)) {
                return false;
            }
            // Ok, so i and j can be fixed with a swap. We have a solution as
            // long as the strings have no more differences.
            j++;
            while (j < s.length()) {
                if (s.charAt(j) != goal.charAt(j)) {
                    return false;
                }
                j++;
            }
            return true;
        }
    }
}
```
