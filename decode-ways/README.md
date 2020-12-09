# Decode Ways

A message containing letters from `A-Z` is being encoded to numbers using the
following mapping:

```
'A' -> 1
'B' -> 2
...
'Z' -> 26
```

Given a **non-empty** string containing only digits, determine the total number
of ways to decode it.

The answer is gauranteed to fit in a **32-bit** integer.

## Hints

1. Note that they don't ask you to provide the decodings, just the counts.
1. Recursion, marching down the input string, will get you a fairly clean
   solution.
1. To get this past the leet grader, you are going to need to use dynamic
   programming to avoid repeated computations.

## Solutions

### Dynamic programming

This is a pretty neat solution. Here recursion is used to branch into the
single character and double character encodings and then the results are
added back together. A memoizer is used to provide a dynamic programming
lookup to eliminate repeated calculations.

```java
class Solution {
    public int numDecodings(String s) {
        Map<Integer, Integer> memo = new HashMap<>();
        return nd(s, 0, memo);
    }

    private int nd(String s, int index, Map<Integer, Integer> memo) {
        // Check the dynamic programming memoizer first.
        if (memo.containsKey(index)) {
            return memo.get(index);
        }

        // Exhausted the string, we have a decoding.
        if (index == s.length()) {
            return 1;
        }

        // Check if invalid decoding.
        char c = s.charAt(index);
        if (c == '0') {
            return 0;
        }

        // Decoding count when consuming the first character.
        int singleDecodings = nd(s, index + 1, memo);
        memo.put(index + 1, singleDecodings);

        // Decoding count when consuming the first two characters.
        int doubleEncodings = 0;
        if (index < s.length() - 1) {
            char c2 = s.charAt(index + 1);
            if (c == '1' || (c == '2' && c2 <= '6')) {
                doubleEncodings = nd(s, index + 2, memo);
            }
        }
        memo.put(index + 2, doubleEncodings);

        return singleDecodings + doubleEncodings;
    }
}
```
