# Longest Word in Dictionary Through Deleting

Given a string `s` and a string array `dictionary`, return the longest string
in the dictionary that can be formed by deleting some of the given string
characters. If there is more than one possible result, return the longest word
with the smallest lexicographical order. If there is no possible result, return
the empty string.

## Hints

1. There seems to be no magic here. Determine if each string is a match, and
   determine if it is the best match seen so far. Return the best match.

## Solutions

### Match beater

The solution here merely iterates through each word and defers to a `match`
function to check for a match. If we have a match, we then check if this is
the best match. At the end we return that match.

If there is any secret sauce here it is in the `match` method. This method
keeps an index into the `candidate` and iterates over the characters of `s`.
In increments the index every time it finds a match at the current position.
We know the `candidate` can be formed by deletes out of `s` if we can move the
index to the end of `candidate`.

The time complexity of this solution is O(n * m) where n is the number of
strings in `dictionary` and m is the number of characters in `s`. The space
complexity is O(1).

```java
class Solution {
    public String findLongestWord(String s, List<String> dictionary) {
        String result = "";
        for (String candidate : dictionary) {
            if (match(candidate, s)) {
                if (candidate.length() > result.length() ||
                    (candidate.length() == result.length() && candidate.compareTo(result) < 0)) {
                    result = candidate;
                }
            }
        }
        return result;
    }

    private boolean match(String candidate, String s) {
        int i = 0;
        int len = candidate.length();
        for (char c : s.toCharArray()) {
            if (candidate.charAt(i) == c) {
                i++;
                if (i == len) {
                    return true;
                }
            }
        }
        return false;
    }
}
```
