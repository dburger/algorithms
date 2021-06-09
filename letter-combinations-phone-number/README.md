# Letter Combinations of a Phone Number

Given a string containing digits from `2-9` inclusive, return all possible
letter combinations that the number could represent. Return the answer in
**any order**.

## Hints

1. This is similar to the [Permutations](../permutations) problems. The twist
   here is that any given output position is determined by a corresponding
   input position.

## Solutions

### SOLUTION TITLE

This solution uses recursion with backtracking to generate all possible
letter combinations. As noted above, this is very similar to typical
permuation generation problems. The usage of the map of input digits to list
of output characters makes this code very similar to those solutions.

```java
class Solution {
    private static final Map<String, List<String>> m = Map.of(
            "2", List.of("a", "b", "c"),
            "3", List.of("d", "e", "f"),
            "4", List.of("g", "h", "i"),
            "5", List.of("j", "k", "l"),
            "6", List.of("m", "n", "o"),
            "7", List.of("p", "q", "r", "s"),
            "8", List.of("t", "u", "v"),
            "9", List.of("w", "x", "y", "z")
        );

    public List<String> letterCombinations(String digits) {
        List<String> acc = new ArrayList<>();
        if (digits.length() > 0) {
            lc(digits, 0, new ArrayList<>(), acc);
        }
        return acc;
    }

    private void lc(String digits, int pos, List<String> word, List<String> acc) {
        if (pos == digits.length()) {
            acc.add(String.join("", word));
            return;
        }
        String c = digits.substring(pos, pos + 1);
        for (String d : m.get(c)) {
            word.add(d);
            lc(digits, pos + 1, word, acc);
            word.remove(word.size() - 1);
        }
    }
}
```
