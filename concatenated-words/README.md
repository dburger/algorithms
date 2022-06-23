# Concatenated Words

Given an array of strings `words` (**without duplicates**), return all the
**concatenated words** in the given list of `words`.

A **concatenated word** is defined as a string that is comprised of at least
two shorter words in the given array.

## Hints

1. A "consumption" approach should work. But how to do?

## Solutions

### Consumption sumption, what's my gumption?

This problem can be solved with what I like to call a consumption approach.
What I mean by this is you attempt to "consume" an input via recursive calls.
If you can consume the input completely, you have found a solution. In this
case the input is each word, and we attempt to consume it by moving via
prefixes that also exist as a word.

While this solution passes the leeter grader it doesn't get a very good
score in the runtime department. The time complexity of this solution is
pretty tricky (TODO:dburger) and I'll leave this as a later exercise.

```java
class Solution {
    public List<String> findAllConcatenatedWordsInADict(String[] words) {
        List<String> result = new ArrayList<>();
        Set<String> wordsSet = new HashSet<>();
        wordsSet.addAll(Arrays.asList(words));
        for (String word : words) {
            wordsSet.remove(word);
            if (find(word, 0, wordsSet)) {
                result.add(word);
            }
            wordsSet.add(word);
        }
        return result;
    }

    private boolean find(String word, int pos, Set<String> words) {
        if (pos == word.length()) {
            return true;
        }
        String prefix = "";
        for (int i = pos; i < word.length(); i++) {
            prefix += word.charAt(i);
            if (words.contains(prefix) && find(word, i + 1, words)) {
                return true;
            }
        }
        return false;
    }
}
```
