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

Here recursion is used to branch into the single and double character
encodings. A memoizer is used to provide a dynamic programming lookup to
eliminate repeated calculations. We get by using an array as the memo.

```java
class Solution {
    public int numDecodings(String s) {
        int[] memo = new int[s.length()];
        Arrays.fill(memo, -1);
        return numDecodings(s, 0, memo);
    }

    private int numDecodings(String s, int offset, int[] memo) {
        if (offset == s.length()) {
            return 1;
        }

        int memoized = memo[offset];
        if (memoized != -1) {
            return memoized;
        }

        char first = s.charAt(offset);
        if (first == '0') {
            return 0;
        }

        int count = numDecodings(s, offset + 1, memo);

        if (offset < s.length() - 1) {
            char second = s.charAt(offset + 1);
            if (first == '1' || (first == '2' && second <= '6')) {
                count += numDecodings(s, offset + 2, memo);
            }
        }

        memo[offset] = count;
        return count;
    }
}
```
