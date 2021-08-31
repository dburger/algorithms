# Replace Words

In English, we have a concept called **root**, which can be followed by some
other word to form another longer word - let's call this word **successor**.
For example, when the **root** `"an"` is followed by the **successor** word
`"other"`, we can form a new word `"another"`.

Given a `dictionary` consisting of many **roots** and a `sentence` consisting
of words separated by spaces, replace all of the **successors** in the sentence
with the **root** forming it. If a **successor** can be replace by more than
one **root**, replace it with the **root** that has **the shortest length**.

Return the `sentence` after the replacement.

## Hints

1. Brute force is pretty straightforward.
1. To beat brute force a specialized data structure can help. Which one?

## Solutions

### Brute force

The brute force method is pretty easy. We need to check if any word in the
`sentence` starts with a word in the `dictionary`. If it does, we replace that
word. Further constraining the problem, if more than one `dictionary` entry
matches the start of a word we use the replacement that is the shortest. This
is easily accomplished by first sorting the list by word length.

This is a brute force solution and thus the time complexity is not nearly as
good as we can do. The time complexity is...complicated. If we take at the
complexity of the `startsWith()` method, the time complexity is O(m * w) where
m is the number of dictionary words and w is the number of words in the
sentence. If we add back in that comparison, we jump to O(m * n * l) where l
is the length of the longest word.

The space complexity is the O(n) where n is the length of
the sentence. This is for the buffer that holds the newly constructed string.

```java
class Solution {
    public String replaceWords(List<String> dictionary, String sentence) {
        Collections.sort(dictionary, (a, b) -> {
            return a.length() - b.length();
        });

        StringBuilder buf = new StringBuilder();
        for (String word : sentence.split(" ")) {
            for (String candidate : dictionary) {
                if (word.startsWith(candidate)) {
                    word = candidate;
                }
            }
            buf.append(word).append(" ");
        }
        int len = buf.length();
        if (len > 0) {
            buf.deleteCharAt(len - 1);
        }
        return buf.toString();
    }
}
```
