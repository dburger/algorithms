# Number of Good Ways to Split a String

You are given a string `s`, a split is called good if you can split `s` into
2 non-empty strings `p` and `q` where its concatenation is equal to `s` and
the number of distinct letters in `p` and `q` are the same.

Return the number of good splits you can make in `s`.

## Hints

1. This boils down to counting unique characters. Multiset and counting map
   will work. Can you do it with a leaner data structure?

## Solutions

### Count me array slots

Since the problem states that the strings only contain lower case letters, the
counting can be done via arrays. Here we count all the characters into the
`right` array. We follow that with a second loop that moves the characters to
the `left` array while maintainging and comparing counts.

The time complexity of this solution is O(n) where n is the length of the
string as there are two loops that iterate through the string. The space
complexity of this solution is O(1). The same sized arrays are made to solve
the problem regardless of the input.

```java
class Solution {
    public int numSplits(String s) {
        int[] left = new int[26];
        int leftChars = 0;
        int[] right = new int[26];
        int rightChars = 0;

        // Add all the characters to right, keeping track
        // of the number of unique characters.
        for (int i = 0; i < s.length(); i++) {
            int pos = s.charAt(i) - 'a';
            int rval = right[pos];
            if (rval == 0) {
                rightChars++;
            }
            right[pos] = rval + 1;
        }

        // Iterate through moving each character to the left
        // keeping track of the number of unique characters.
        int count = 0;
        for (int i = 0; i < s.length() - 1; i++) {
            int pos = s.charAt(i) - 'a';
            int lval = left[pos];
            if (lval == 0) {
                leftChars++;
            }
            left[pos] = lval + 1;
            int rval = right[pos];
            if (rval == 1) {
                rightChars--;
            }
            right[pos] = rval - 1;
            if (leftChars == rightChars) {
                count++;
            }
        }

        return count;
    }
}
```
