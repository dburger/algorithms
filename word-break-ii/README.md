# Word Break II

Given a string `s` and a dictionary of strings `wordDict`, add spaces in `s` to
construct a sentence where each word is a valid dictionary word. Return all
such possible sentences in **any order**.

**Note** that the same word in the dictionary may be reused multiple times in
the segmentation.

## Hints

1. This is an example of a classic "consume and backtrack" style problem. I
   wonder if there is a standard name for these.

## Solutions

### Consume, backtrack, recurse recurse

There are many problems that fall into this same solution category. They are
solved by attempting to consume the input. You keep track of where you are in
the input and recurse. If you reach the end you have a solution. You always
remove the last consumption and attempt with the next option. This is the
backtracking.

```java
class Solution {
    public List<String> wordBreak(String s, List<String> wordDict) {
        List<String> acc = new ArrayList<>();
        wordBreak(s, 0, wordDict, new ArrayList<>(), acc);
        return acc;
    }

    private void wordBreak(String s, int pos, List<String> wordDict, List<String> parts, List<String> acc) {
        if (pos == s.length()) {
            acc.add(String.join(" ", parts));
            return;
        }

        for (String word : wordDict) {
            if (s.indexOf(word, pos) == pos) {
                parts.add(word);
                wordBreak(s, pos + word.length(), wordDict, parts, acc);
                parts.remove(parts.size() - 1);
            }
        }
    }
}
```
