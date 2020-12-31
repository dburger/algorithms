# Can Construct

Write a function `canConstruct(target, wordBank)` that accepts a `target`
string and an array of strings. The function should return a boolean indicating
whether or not the `target` can be constructed by concatenating elements of the
`wordBank` array. You may reuse elements of `wordBank` as many times as needed.

## Hints

1. This problem can be solved using recursion that attempts to match at the
   front of the string and then recurses with that prefix removed. The full
   match base case is reached if the string is exhausted.

## Solutions

### Recursive target reduction

This solution follows the hint above. It uses recursion, and attempts to
repeatedly match a prefix and recurse on a smaller string. Note the use
of `index` here to avoid having to make new strings but instead point at
the working location in the one and only string.

```java
class Solution {
    public boolean canConstruct(String target, String[] wordBank) {
        return canConstruct(target, 0, wordBank);
    }

    private boolean canConstruct(String target, int index, String[] wordBank) {
        if (index == target.length()) {
            return true;
        }

        for (String w : wordBank) {
            if (target.indexOf(w, index) == index) {
                if (canConstruct(target, index + w.length(), wordBank)) {
                    return true;
                }
            }
        }

        return false;
    }
}
```

### Memoized recursive target reduction

Like the solution above, but with a memoizer introduced for speed.

```java
class Solution {
    public boolean canConstruct(String target, String[] wordBank) {
        return canConstruct(target, 0, wordBank, new HashMap<>());
    }

    private boolean canConstruct(String target, int index, String[] wordBank, Map<Integer, Boolean> memo) {
        if (memo.containsKey(index)) {
            return memo.get(index);
        } else if (index == target.length()) {
            return true;
        }

        for (String w : wordBank) {
            if (target.indexOf(w, index) == index) {
                if (canConstruct(target, index + w.length(), wordBank, memo)) {
                    memo.put(index, true);
                    return true;
                }
            }
        }

        memo.put(index, false);
        return false;
    }
}
```

### Dynamic programming table approach

The classic dynamic programming table approach. The value at `table[i]`
indicates if the prefix up to character `i` can be constructed. As we
advance through the table we look back the length of each candidate for
a match plus the existence of that candidate extending up to `i`.

```java
class Solution {
    public boolean canConstruct(String target, String[] wordBank) {
        boolean[] table = new boolean[target.length() + 1];
        // Empty string can be formed by selecting no words from wordbank.
        table[0] = true;

        for (int i = 1; i <= target.length(); i++) {
            for (String w : wordBank) {
                int prior = i - w.length();
                if (prior >= 0 && table[prior] && target.indexOf(w, prior) == prior) {
                    table[i] = true;
                    // We don't need to check any other candidates, we already found one
                    // that flips table[i] to true.
                    break;
                }
            }
        }

        return table[target.length()];
    }
}
```
