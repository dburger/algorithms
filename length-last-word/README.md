# Length of Last Word

Given a string `s` consisting of some words separated by spaces, return the
length of the last word in the string. If the last word does not exist return
`0`.

A **word** is a maximal substring consisting of non-space characters only.

## Hints

1. Easier to walk backwards to get last word.

## Solutions

### Walk and talk

Kind of a mini-state machine with `wordEnd` defining the state of whether or
not a word has been seen yet.

```java
class Solution {
    public int lengthOfLastWord(String s) {
        // Keep track of word end, when -1, no word seen yet.
        int wordEnd = -1;
        // Walk through the string backwards.
        for (int i = s.length() - 1; i > -1; i--) {
            char c = s.charAt(i);
            if (wordEnd > 0 && c == ' ') {
                // If we have already started the word and see space,
                // we are done.
                return wordEnd - i;
            } else if (wordEnd == -1 && c != ' ') {
                // Started the last word.
                wordEnd = i;
            }
        }
        // If started, word ran to beginning, otherwise no word.
        return wordEnd > -1 ? wordEnd + 1 : 0;
    }
}
```
