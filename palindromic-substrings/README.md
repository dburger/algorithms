# Palindromic Substrings

Given a string `s`, return the number of **palindromic substrings** in it.

A string is a **palindrome** when it reads the same backward as forward.

A **substring** is a contiguous sequence of characters within the string.

## Hints

1. Check from starting with 1, check with starting from 2.

## Solutions

### A one and a two and a ....

This solution merely counts all the palindromic substrings by doing a round of
checks from starting with one character and then a round of check from starting
with two characters. The approach is very simple.

The time complexity of this approach is O(n^2). That is we do `n` checks where
`n` is the length of the input string where each check can iterate up to the
full length of the string. We do this twice, once for the single character
starts and once for the two character starts. The space complexity for this
solution is O(1). The space allocated is not dependent on the input.

```java
class Solution {
    public int countSubstrings(String s) {
        int count = 0;
        for (int i = 0; i < s.length(); i++) {
            count += countPalindromes(s, i, i);
        }

        for (int i = 0; i < s.length() - 1; i++) {
            count += countPalindromes(s, i, i + 1);
        }

        return count;
    }

    private int countPalindromes(String s, int start, int end) {
        int count = 0;
        while (start > - 1 && end < s.length() && s.charAt(start) == s.charAt(end)) {
            count++;
            start--;
            end++;
        }
        return count;
    }
}
```
