# Minimum Window Substring

Given two strings `s` and `t`, return the minimum window is `s` which contains
all the characters in `t`. If there is no such window in `s` that covers all
characters in `t`, return the empty string `""`.

**Note** that if there is such a window, it is gauranteed that there will always
be only one unique minimum window in `s`.

## Hints

1. Think sliding window.

## Solutions

### Sliding window

This is the usual sliding window approach. A map (essentially a multiset) is
used to help track whether or not you have a match. hi and lo indices are used
to track the window. The hi is expanded until you have a match, and then lo is
expanded to see how far you can constrain and still have a match.

```java
class Solution {
    public String minWindow(String s, String t) {
        if (s.isEmpty() || t.isEmpty()) {
            return "";
        }

        // Build up a multiset of what is needed.
        Map<Character, Integer> needs = new HashMap<>();
        for (char c : t.toCharArray()) {
            needs.put(c, needs.getOrDefault(c, 0) + 1);
        }
        Map<Character, Integer> has = new HashMap<>();

        // Markers for index positions.
        int bestLo = -1;
        int bestHi = -1;
        int lo = 0;
        int hi = 0;

        // Variables to keep track of characters matches.
        int required = needs.size();
        int matched = 0;

        for (; hi < s.length(); hi++) {
            char c = s.charAt(hi);
            Integer needsVal = needs.get(c);
            if (needsVal == null) {
                // Not needed, ignore.
                continue;
            }

            Integer hasVal = has.put(c, has.getOrDefault(c, 0) + 1);
            hasVal = hasVal == null ? 1 : hasVal + 1;
            if (hasVal.equals(needsVal)) {
                matched++;
            }

            if (matched != required) {
                // We don't have a full match yet, continue to expand hi.
                continue;
            }

            // We have a match, move lo to the right as far as possible while
            // updating the has map. When it no longer is a match, break out
            // to continue updating hi to the right.
            for (;;) {
                char r = s.charAt(lo++);
                Integer count = has.get(r);
                if (count != null) {
                    count--;
                    if (count == 0) {
                        has.remove(r);
                        matched--;
                        break;
                    } else {
                        has.put(r, count);
                        if (count < needs.get(r)) {
                            matched--;
                            break;
                        }
                    }
                }
            }

            if (bestLo == -1 || hi + 1 - lo + 1 < bestHi - bestLo) {
                bestLo = lo - 1;
                bestHi = hi + 1;
            }
        }
        return bestLo == -1 ? "" : s.substring(bestLo, bestHi);
    }
}
```
