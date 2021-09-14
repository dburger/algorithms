# Reconstruct Original Digits from English

Given a string `s` containing an out-of-order English representation of the
digits `0-9`, return the digits in **ascending** order.

## Hints

1. There is a uniqueness hierarchy buried in the characters of the digits. For
   example, "zero" is the only digit with a "z". Could you reconstruct the
   digits from this?

## Solutions

### Follow ye hierarchy

This solution follows the hint. It uses a hierarchy of the characters within
the digits. A good chain is the "four" -> "five" -> "seven" sequence. "four"
is the only digit with a "u". After the characters for the four are removed,
"five" is the only remaining digit with an "f". After the characters of the
five are removed "seven" is the only digit with a "v".

Here we use this hierarchy as laid out here:

```
char  - identified by
zero  - z
two   - w
four  - u
eight - g
six   - x
one   - o after zero, two and four removal
three - t after two and eight removal
five  - f after four removal
seven - v after five removal
nine  - n after one and seven removal
```

Here we follow the hiearchy and `wipe` out the necessary characters indicating
the "consumption" of that character. Note that we don't have to wipe out the
entire string. For example, none of these calculations look at the "i" so we
don't bother wiping that value.

As the number of digits is `10` and their lengths are constant the time
complexity of this solution is O(n), where n is the length of the input
string.

```java
class Solution {
    public String originalDigits(String s) {
        int[] charCounts = new int[26];
        for (int i = 0; i < s.length(); i++) {
            charCounts[s.charAt(i) - 'a']++;
        }

        int[] digitCounts = new int[10];
        digitCounts[0] = wipe('z', "zo", charCounts);
        digitCounts[2] = wipe('w', "two", charCounts);
        digitCounts[4] = wipe('u', "fou", charCounts);
        digitCounts[8] = wipe('g', "gt", charCounts);
        digitCounts[6] = wipe('x', "x", charCounts);
        digitCounts[1] = wipe('o', "on", charCounts);
        digitCounts[3] = wipe('t', "t", charCounts);
        digitCounts[5] = wipe('f', "fv", charCounts);
        digitCounts[7] = wipe('v', "vn", charCounts);
        digitCounts[9] = wipe('n', "nn", charCounts);

        StringBuilder buf = new StringBuilder();
        for (int i = 0; i < digitCounts.length; i++) {
            int count = digitCounts[i];
            while (count > 0) {
                buf.append(i);
                count--;
            }
        }

        return buf.toString();
    }

    private int wipe(char c, String digit, int[] counts) {
        int pos = c - 'a';
        int count = 0;
        while (counts[pos] > 0) {
            for (int i = 0; i < digit.length(); i++) {
                counts[digit.charAt(i) - 'a']--;
            }
            count++;
        }
        return count;
    }
}
```

### You don't really need to wipe

Ok, in the real world you always need to wipe. Here, you can get away without
it. Instead of keeping track of the characters that are left we can merely
reference the prior determined counts in the hierarchy. This makes for much
simpler code but no change in time and space complexity.

```java
class Solution {
    public String originalDigits(String s) {
        int[] charCounts = new int[26];
        for (int i = 0; i < s.length(); i++) {
            charCounts[s.charAt(i) - 'a']++;
        }

        int[] digitCounts = new int[10];

        digitCounts[0] = charCounts['z' - 'a'];
        digitCounts[2] = charCounts['w' - 'a'];
        digitCounts[4] = charCounts['u' - 'a'];
        digitCounts[8] = charCounts['g' - 'a'];
        digitCounts[6] = charCounts['x' - 'a'];

        digitCounts[1] = charCounts['o' - 'a'] - digitCounts[0] - digitCounts[2] - digitCounts[4];
        digitCounts[3] = charCounts['t' - 'a'] - digitCounts[2] - digitCounts[8];
        digitCounts[5] = charCounts['f' - 'a'] - digitCounts[4];
        digitCounts[7] = charCounts['v' - 'a'] - digitCounts[5];
        digitCounts[9] = (charCounts['n' - 'a'] - digitCounts[1] - digitCounts[7]) / 2;

        StringBuilder buf = new StringBuilder();
        for (int i = 0; i < digitCounts.length; i++) {
            int count = digitCounts[i];
            while (count > 0) {
                buf.append(i);
                count--;
            }
        }

        return buf.toString();
    }
}
```
