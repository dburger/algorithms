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

### Dynamic programming table

This is a bit tricky and not as fast as the above solution. I think there may
be a seed of a good idea here but haven't fleshed it out here fully. Here we
build a table of rows and columns where `(r, c) == 1` only if the subtring
from r to c can be achieved by some concatention of strings in `wordDict`.
Determining this becomes determining if:

1. The subtring from r to c is a word.
1. r is the start of the entire string, or, there is a 1 somewhere in the
   rth column. That is another sequence of strings matches that leads up
   to this one.

```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        Set<String> words = new HashSet<>(wordDict);
        int n = s.length();
        boolean[][] dp = new boolean[n + 1][n + 1];
        for (int r = 0; r < n + 1; r++) {
          for (int c = r + 1; c < n + 1; c++) {
            String part = s.substring(r, c);
            if (words.contains(part)) {
              dp[r][c] = (r == 0 || hastrue(dp, r));
            }
          }
        }
      return hastrue(dp, n);
    }
    private boolean hastrue(boolean[][] dp, int c) {
      for (int r = 0; r < dp.length && r < c; r++) {
        if (dp[r][c]) {
          return true;
        }
      }
      return false;
    }
}
```
