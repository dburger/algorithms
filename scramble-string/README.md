# Scramble String

We can scramble a string `s` to get a string `t` using the following algorithm.

1. If the lenght of the string is 1, stop.
1. If the length of the string is > 1, do the following:
   * Split the string into two non-empty substrings at random index, i.e., if
     the string is `s`, divide it to `x` and `y` where `s = x + y`.
   * **Randomly** decide to swap the two substrings or to keep them in the same
     order. i.e., after this step, `s` may become `s = x + y` or `s = y + x`.
   * Apply step 1 recursively on each of the two substrings `x` and `y`.

Given two strings `s1` and `s2` of **the same length**, return `true` if `s2` is
a scrambled string of `s1`, oetherwise, return false.

## Hints

1. On the surface this problem seems very tricky. Think of `isScramble` being
   recursive with a loop that fires off recursive invocations. In the loop
   `s1` is split at every position, and recursive calls can be made with the
   invocation calling `isScramble` with those splits swapped and not swapped.
1. A naive implementation can just check for a match by comparing strings. If
   they are equal, you are done. If they are not equal you can recurse.
   But wait! If they do not have the same characters in them there is no
   possible way to make them equivalent by scrambling. Thus you can add code
   to exit early in this case.

## Solutions

### Recursive scrambler

This solution mainly follows the hints from above. Note the string maniuplations
in the swap versus no swap cases where we are merely making sure that we have
the correct length strings to match the splits.

```java
class Solution {
    public boolean isScramble(String s1, String s2) {
        if (s1.equals(s2)) {
            return true;
        }

        // We can stop earlier if we see that the characters in
        // s1 and s2 don't match - no scramble could work.
        int[] charFreq = new int[26];
        for (int i = 0; i < s1.length(); i++) {
            charFreq[s1.charAt(i) - 'a']++;
            charFreq[s2.charAt(i) - 'a']--;
        }
        for (int i = 0; i < charFreq.length; i++) {
            if (charFreq[i] != 0) {
                return false;
            }
        }

        for (int i = 1; i < s1.length(); i++) {
            String s1Prefix = s1.substring(0, i);
            String s1Suffix = s1.substring(i);

            // These are the strings from s2 for comparison if we
            // do not swap. That is, the lengths match those of
            // the s1{Prefix,Suffix}.
            String s2PrefixNoSwap = s2.substring(0, i);
            String s2SuffixNoSwap = s2.substring(i);

            boolean result = isScramble(s1Prefix, s2PrefixNoSwap) &&
                isScramble(s1Suffix, s2SuffixNoSwap);
            if (result) {
                return true;
            }

            // These are the strings from s2 for comparison if we
            // do swap. That is, the lengths are opposite of
            // the s1{Prefix,Suffix}.
            String s2PrefixSwap = s2.substring(0, s1Suffix.length());
            String s2SuffixSwap = s2.substring(s1Suffix.length());

            result = isScramble(s1Suffix, s2PrefixSwap) &&
                isScramble(s1Prefix, s2SuffixSwap);
            if (result) {
                return true;
            }
        }
        return false;
     }
}
```
