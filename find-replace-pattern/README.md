# Find and Replace Pattern

You have a list of `words` and a `pattern`, and you want to know which words in
`words` matches the pattern.

A word matches the pattern if there exists a permutation of letters `p` so that
after replace every letter in `x` in the pattern with `p(x)`, we get the desired
word.

*(Recall that a permutation of letters is a bijection from letters to letters:
every letter maps to another letter, and no two letters map to the same
letter.)*

Return a list of the words in `words` that match the given pattern.

You may return the answer in any order.

## Hints

1. Iterating from the start of word and pattern and making mappings as needed
   should do the trick. When you encounter a character already mapped but
   mapped to the wrong character you can reject that word.

## Solutions

### Mappings

This follows the hint. We iterate through the `pattern` mapping characters to
the needed target as we go. If we find that:

* the pattern character has already been mapped to something else
* the word character has already been mapped from something else

we reject that word and continue to the next one.

Because of the limits on the input characters these mappings are easily handled
with `char[]` and `boolean[]` and actual hash maps are not required.

The time complexity of this problem is O(m * n) where m is the number of words
and n is the length of the pattern. This can be easily seen from the outer loop
on words and the inner loop over the length of the pattern. The space complexity
is O(n) where n is the length of the pattern. We allocate character arrays the
length of this pattern to do the processing.

```java
class Solution {
    public List<String> findAndReplacePattern(String[] words, String pattern) {
        List<String> results = new ArrayList<>();
        char[] pchar = pattern.toCharArray();
        for (String w : words) {
            if (w.length() != pchar.length) {
                continue;
            }

            char[] wchar = w.toCharArray();
            char[] translations = new char[26];
            boolean[] used = new boolean[26];
            boolean works = true;

            for (int i = 0; i < pchar.length; i++) {
                char pc = pchar[i];
                char wc = wchar[i];
                char mapped = translations[pc - 'a'];
                if (mapped != wc) {
                    if (mapped != 0) {
                        // Already mapped to something else.
                        works = false;
                        break;
                    }
                    if (used[wc - 'a']) {
                        // Already used by another pchar character.
                        works = false;
                        break;
                    }
                    translations[pc - 'a'] = wc;
                    used[wc - 'a'] = true;
                }
            }
            if (works) {
                results.add(w);
            }
        }
        return results;
    }
}
```
