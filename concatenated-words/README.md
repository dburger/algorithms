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

### Trie again

Looking for the slow spot in the previous solution gives some hints on how to
speed things up. Such a slow spot is when we check if a `prefix` is a string
in `words`. We continually grow that prefix and check until we either find a
match or exhaust the string. Obviously given a `prefix` if none of `words`
starts with that value we could just quit there. We can avoid growing the
prefix to any longer lengths with their subsequent checks. Our `Set` doesn't
provide for this kind of check. So what data structure allows us to do this?
The `Trie` data structure does.

The solution proceeds by first building up the `Trie`. Then we iterate through
words and check if the word is a concatenation of the words in the `Trie`.
This is done with a `Trie` instance method that uses recursion and passes the
position within the `word` we are trying to match from. In any recursive call
if we run off the bottom of the `Trie` we know that a match is not possible and
we can stop that recursion invocation.

```java
class Solution {
    private class Node {
        String word = null;
        private final Node[] nodes = new Node[26];
    }

    private class Trie {
        private final Node root = new Node();

        Trie insert(String word) {
            Node curr = root;
            for (char c : word.toCharArray()) {
                Node n = curr.nodes[c - 'a'];
                if (n == null) {
                    n = new Node();
                    curr.nodes[c - 'a'] = n;
                }
                curr = n;
            }
            curr.word = word;
            return this;
        }

        Trie insertAll(String[] words) {
            for (String word : words) {
                this.insert(word);
            }
            return this;
        }

        boolean isConcat(String word, int pos) {
            if (pos == word.length()) {
                return true;
            }
            Node curr = this.root;
            while (curr != null && pos < word.length()) {
                curr = curr.nodes[word.charAt(pos) - 'a'];
                if (curr == null) {
                    break;
                }
                if (curr.word != null && !curr.word.equals(word) && isConcat(word, pos + 1)) {
                    return true;
                }
                pos++;
            }
            return false;
        }
    }

    public List<String> findAllConcatenatedWordsInADict(String[] words) {
        List<String> result = new ArrayList<>();
        Trie t = new Trie().insertAll(words);
        for (String word : words) {
            if (t.isConcat(word, 0)) {
                result.add(word);
            }
        }
        return result;
    }
}
```
