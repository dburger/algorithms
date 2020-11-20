# Permuation in String

Given two strings `s1` and `s2`, write a function to return true if `s2`
contains the permutation of `s1`. In other words, one of the first string's
permutations is the **substring** of the second string.

## Hints

1. You could generate all the permutations of `s1` and then search for those
   in `s2`. Generating the permutations is hella expensive, O(n!).
1. You could sort `s1` and then compare to sorted segments of `s2`. Again, this
   is a horribly slow solution as each sort is O(n*log(n)) where n is the
   length of the string you are sorting.
1. Sliding window, with a way to keep track of the matching of the current
   window.

## Solutions

### Sliding window with map

This solution uses a sliding window and keeps track of the counts in a map.

```java
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        if (s1.isEmpty() || s2.isEmpty() || s2.length() < s1.length()) {
            return false;
        }

        // Build up the multiset of what counts are needed.
        Map<Character, Integer> needs = new HashMap<>();
        for (char c : s1.toCharArray()) {
            needs.put(c, needs.getOrDefault(c, 0) + 1);
        }

        // Will keep track of current counts in the window.
        Map<Character, Integer> has = new HashMap<>();

        // Tracks what we need to match and current match.
        int required = needs.size();
        int matched = 0;

        // Start of the sliding window.
        int start = 0;
        for (int i = 0; i < s2.length(); i++) {
            char c = s2.charAt(i);
            Integer needsCount = needs.get(c);
            if (needsCount == null) {
                // If the character at the end of the window isn't needed
                // we don't have a contiguous run of needed characters,
                // start over.
                start = i + 1;
                has.clear();
                matched = 0;
            } else {
                // The end of the window has a needed character.
                Integer prior = has.put(c, has.getOrDefault(c, 0) + 1);
                if (prior == null) {
                    prior = 0;
                }
                // But if it takes the count over the needed count, move
                // the start of the window until it has the right count for
                // the new character.
                while (has.get(c) > needsCount) {
                    char r = s2.charAt(start++);
                    if (needs.containsKey(r)) {
                        if (has.put(r, has.get(r) - 1) == needs.get(r)) {
                            // If the old value was equal to what is needed, we are now
                            // under and no longer match.
                            matched--;
                        }
                    }
                }
                if (prior.equals(needsCount - 1)) {
                    // If the old value was one under the needed count, we advanced that
                    // to another match.
                    matched++;
                }
            }
            if (matched == required) {
                return true;
            }
        }
        return false;
    }
}
```

### Sliding window with array

```java
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        if (s1.isEmpty() || s2.isEmpty() || s2.length() < s1.length()) {
            return false;
        }

        // Build up the array of what counts are needed and character matches required.
        int[] needs = new int[26];
        int required = 0;
        for (char c : s1.toCharArray()) {
            if (needs[c - 'a']++ == 0) {
                required++;
            }
        }

        // Will be used to keep track of current counts in the window.
        int[] has = new int[26];
        int matched = 0;

        // left is left side of window, hi is right.
        for (int left = 0, right = 0; right < s2.length(); right++) {
            int i = s2.charAt(right) - 'a';
            if (needs[i] == 0) {
                // Hit a character that is not needed, clear out matches and move left past right.
                for (int j = left; j < right; j++) {
                    int i2 = s2.charAt(j) - 'a';
                    if (needs[i2] > 0) {
                        has[i2]--;
                    }
                }
                left = right + 1;
                matched = 0;
            } else {
                // Hit a character that is needed. If it pushes the count for that character
                // too high, move the left side of the window until it is acceptable again.
                if (has[i]++ == needs[i] - 1) {
                    matched++;
                }
                while (has[i] > needs[i]) {
                    int i2 = s2.charAt(left++) - 'a';
                    if (needs[i2] > 0 && has[i2]-- == needs[i2]) {
                        matched--;
                    }
                }
            }

            if (matched == required) {
                return true;
            }
        }

        return false;
    }
}
```
