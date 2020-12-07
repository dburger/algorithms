# Word Break

Given a **non-empty** string `s` and a dictionary `wordDict` containing a list
of **non-empty** words, determine if `s` can be segmented into a
space-separated sequence of one or more dictionary words.

**Note:**

* The same word in the dictionary may be reused multiple times in the
  segmentation.
* You may assume the dictionary does not contain duplicate words.

## Hints

1. This can be done in the approach of finding matches at either the start or
   the end of the string, and then attempting to "consume" the entire string.
   This works really nicely with recursion.
1. Pertaining to the hint above, if you don't keep track of strings that don't
   match, you will end up doing a lot of double work. Providing a rejection
   memoizer speeds this up.

## Solutions

### Consume and track the rejects

Here we attempt to match at the start of the string and then recurse
with the partially matched string. Without the `rejects` set, this code
hits the time limit on leeter.

```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        Set<String> rejects = new HashSet<>();
        return wb(s, wordDict, rejects);
    }
    private boolean wb(String s, List<String> words, Set<String> rejects) {
        if (s.isEmpty()) {
            return true;
        }
        if (rejects.contains(s)) {
            return false;
        }
        for (String word : words) {
            if (s.startsWith(word)) {
                String part = s.substring(word.length());
                boolean result = wb(part, words, rejects);
                if (result) {
                    return true;
                } else {
                    rejects.add(part);
                }
            }
        }
        return false;
    }
}
```
