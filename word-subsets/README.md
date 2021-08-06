# Word Subsets

You are given two string arrays `words1` and `words2`.

A string `b` is a **subset** of string `a` if every letter in `b` occurs in
`a` including multiplicity.

*   For example, `"wrr"` is a subset of `"warrior"` but is not a subset of
    `"world"`.

A string `a` from `words1` is **universal** if for every string `b` in
`words2`, `b` is a subset of `a`.

Return an array of all **universal** strings in `words1`. You may return the
answer in **any order**.

## Hints

1. Brute force solutions could store character counts for each word and then
   iterate through checking each word in `words1` against all the words in
   `words2` to determine universal words. This works, but is O(m * n) in
   number of comparisons and will fail the leeter's timer.
1. How can we get away from brute force? Well, a word in `words1` is defined to
   be universal if it is a superset of each word in `words2`. Instead of
   comparing each separately we can make a single character count that contains
   the max count for each character across the words in `words2`. This allows
   for a superset check with a comparison of two character count structures
   instead of an iteration across each element of `words2`.

## Solutions

### Merge and splurge

This solution follows the idea of the second hint. Character count structure
`max` is computed as the max character counts across all words in `words2`.
This makes it possible to determine if each word in `words1` is a superset by:

1. Computing the word's character count.
1. Checking if for each character in `max`'s count structure, the word's
   character count is higher. If it is, the word is universal.

```java
class Solution {
    public List<String> wordSubsets(String[] words1, String[] words2) {
        int[] max = new int[26];
        for (String w : words2) {
            merge(counts(w), max);
        }

        List<String> results = new ArrayList<>();
        for (String w : words1) {
            int[] counts = counts(w);
            if (covers(counts, max)) {
                results.add(w);
            }
        }
        return results;
    }

    private int[] counts(String word) {
        int[] counts = new int[26];
        for (char c : word.toCharArray()) {
            counts[c - 'a']++;
        }
        return counts;
    }

    private void merge(int[] source, int[] dest) {
        for (int i = 0; i < dest.length; i++) {
            dest[i] = Math.max(source[i], dest[i]);
        }
    }

    private boolean covers(int[] candidate, int[] max) {
        for (int i = 0; i < candidate.length; i++) {
            if (max[i] > candidate[i]) {
                return false;
            }
        }
        return true;
    }
}
```
