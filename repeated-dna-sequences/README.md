# Repeated DNA Sequences

The **DNA sequence** is composed of a series of nucleotides abbreviated as `'A'`, `'C'`,
`'G'`, and `'T'`. For example, `"ACGAATTCCG"` is a **DNA sequence**.

When studying **DNA**, it is useful to identify repeated sequences within the DNA.

Given a string `s` that represents a **DNA sequence**, return all the 10 letter long sequences
(substrings) that occur more than once in a DNA molecule. You may return the answer in **any
order**.

## Hints

1. A counting map is an obvious approach.

## Solutions

### Brute force counting map

Nothing fancy here, just an interative loop that updates a counting map. This is followed by
a final loop that collects up the sequences with a count greater than 1.

This solution is O(n) where n is the length of `s`. The space complexity is determined by the
size of the returned list. This is also O(n) as the upper bound on repeats is the length of
the string minus 10.

Note that this is listed as a leeter medium problem. The solution here would seem to fall
squarely into the easy camp. If they wanted to mark this one medium they probably should
have made sure this approach would fail the leeter's timers.

```java
class Solution {
    public List<String> findRepeatedDnaSequences(String s) {
        Map<String, Integer> counts = new HashMap<>();
        for (int i = 0; i <= s.length() - 10; i++) {
            String sub = s.substring(i, i + 10);
            counts.put(sub, counts.getOrDefault(sub, 0) + 1);
        }
        List<String> result = new ArrayList<>();
        for (Map.Entry<String, Integer> e : counts.entrySet()) {
            if (e.getValue() > 1) {
                result.add(e.getKey());
            }
        }
        return result;
    }
}
```
