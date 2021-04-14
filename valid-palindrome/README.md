# Valid Palindrome

Given a string `s`, determine if it is a palindrome, considering only
alphanumeric characters and ignoring cases.

## Hints

1. This is a slight twist on the usual palindrome question in that it allows
   there to be characters that are thrown out of consideration.

## Solutions

### Index pointers that
[meeeeeet in the middle](https://www.youtube.com/watch?v=qWKpCmPdGmM)

Here we set an index pointer to the front and end of the string, peel off a
character at a time, comparing them and advancing the pointers as necessary.

The time complexity for this solution is O(n), where n is the length of the
string. The space complexity is O(1).

```java
class Solution {
    public boolean isPalindrome(String s) {
        int n = s.length();
        if (n < 2) {
            return true;
        }
        int i = 0;
        n--;
        while (i < n) {
            char lo = s.charAt(i);
            char hi = s.charAt(n);
            boolean loValid = valid(lo);
            boolean hiValid = valid(hi);
            if (!loValid && !hiValid) {
                i++;
                n--;
                continue;
            } else if (!loValid) {
                i++;
                continue;
            } else if (!hiValid) {
                n--;
                continue;
            } else if (Character.toLowerCase(lo) != Character.toLowerCase(hi)) {
                return false;
            }
            i++;
            n--;
        }
        return true;
    }

    private boolean valid(char c) {
        return Character.isAlphabetic(c) || Character.isDigit(c);
    }
}
```
