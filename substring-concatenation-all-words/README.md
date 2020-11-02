# Substring With Concatentation of all Words

You are given a string `s` and an array of strings `words` of
**the same length**. Return all starting indices of substring(s) in `s` that is
a concatenation of each word in `words` **exactly once, in any order**, and
**without any intervening characters**.

You can return the answer in **any order**.

## Hints

1. Use the fact that all words are of the same length to your advantage. That
   is, a map of the words can be made, and the input string can be chunked in
   those same lengths.

## Solutions

### Two maps is nice

In this solution a `wordsMap` map is made from word to the count of that word.
Then when searching the input string `s`, a map is built of the word chunk to
the count for that chunk in `stringMap`. Of course if the chunk is not `wordsMap`
you bail immediately. At the end if `wordsMap` and `stringMap` are the same then
you have found a match.

```java
class Solution {
    public List<Integer> findSubstring(String s, String[] words) {
        // First build a count map for words.
        Map<String, Integer> wordMap = new HashMap<>();
        for (String w : words) {
            wordMap.put(w, wordMap.getOrDefault(w, 0) + 1);
        }

        int wordLen = words[0].length();
        int wordsLen = wordLen * words.length;

        // Loop through the input string.
        List<Integer> indices = new ArrayList<>();
        for (int i = 0; i < s.length() - wordsLen + 1; i++) {
            Map<String, Integer> stringMap = new HashMap<>();
            for (int j = 0; j < words.length; j++) {
                int index = i + (j * wordLen);
                // See if you have a match for the next chunk.
                String part = s.substring(index, index + wordLen);
                if (wordMap.containsKey(part)) {
                    // Match, keep track of the count.
                    Integer prior = stringMap.put(part, stringMap.getOrDefault(part, 0) + 1);
                    // If going over the count, bail.
                    if (prior != null && prior == wordMap.get(part)) {
                        break;
                    }
                } else {
                    // No match, bail.
                    break;
                }
            }
            // If the two maps are equal, we have found a match.
            if (stringMap.equals(wordMap)) {
                indices.add(i);
            }
        }

        return indices;
    }
}
```
