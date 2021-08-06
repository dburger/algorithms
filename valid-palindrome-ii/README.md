# Valid Palindrome II

Given a string `s`, return `true` if the `s` can be a palindrome after deleting
**at most one** character from it.

## Hints

1. If you have a drop one character palindrome implementation and a standard
   palindrome implementation, the two can be combined to knock this out?

## Solutions

### Double your palindromes, double the fun

This solution follows the hint. Two slightly different palindrome
implementations are used. The first one will drop a character and differ to
the second standard implementation one the first character mismatch it finds.

The time complexity of this solution is O(n) where n is the length of `s`. The
space complexity is O(1).

```java
class Solution {
    public boolean validPalindrome(String s) {
        int lo = 0;
        int hi = s.length() - 1;
        while (lo < hi) {
            if (s.charAt(lo) == s.charAt(hi)) {
                lo++;
                hi--;
            } else {
                String left = s.substring(0, lo) + s.substring(lo + 1);
                String right = s.substring(0, hi) + s.substring(hi + 1);
                return vp(left) || vp(right);
            }
        }
        return true;
    }

    private boolean vp(String s) {
        int lo = 0;
        int hi = s.length() - 1;
        while (lo < hi) {
            if (s.charAt(lo) != s.charAt(hi)) {
                return false;
            }
            lo++;
            hi--;
        }
        return true;
    }
}
```

### Double you palindromes, but don't cover the same ground twice

Here we tweak the prior solution for speed. Note that in the prior solution
we iterate in till we determine if we have a palindrome or find a mismatch. In
the mismatch case, new strings are constructed. `left` is constructed to keep
the left mismatching character and drop the right. `right` is constructed to
keep the right mismatching character and drop the left. These are then passed
to the standard palindrome implementation in the line
`return vp(left) || vp(right)`. Two unnecessary slow downs occur here. First,
constructing new strings in Java is slightly expensive. It would be faster to
pass the same string but instead pass indices indicating what to look at.
Second, the standard implemenation starts over at the head and tail of the
string, when some portion of this may have already been checked. Passing
indices indicating where to perform the work can solve this problem as well.

This solution ends up being time complexity O(n) and space complexity O(1).
This is the same big O speed as the prior solution however in practice these
tweaks make a noticable difference to leeter's grader.

```java
class Solution {
    public boolean validPalindrome(String s) {
        int lo = 0;
        int hi = s.length() - 1;
        while (lo < hi) {
            if (s.charAt(lo) == s.charAt(hi)) {
                lo++;
                hi--;
            } else {
                return vp(s, lo, hi - 1) || vp(s, lo + 1, hi);
            }
        }
        return true;
    }

    private boolean vp(String s, int lo, int hi) {
        while (lo < hi) {
            if (s.charAt(lo) != s.charAt(hi)) {
                return false;
            }
            lo++;
            hi--;
        }
        return true;
    }
}
```
