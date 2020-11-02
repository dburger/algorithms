# ZigZag Conversion

The string `"PAYPALISHIRING"` is written in a zigzag pattern on a given number
of rows like this:

```text
P A H N
APLSIIG
Y I R
```

And then read line by line: `"PAHNAPLSIIGYIR"`.

Wite the code that will take a string and make this conversion given a number of
rows.

## Hints

1. Try the down "zig" as one loop, and the diagonal "zag" as another, until you
   exhaust all the characters. Each line is accumulated into a different string.

## Solutions

### Two loops within an outer loop

```java
class Solution {
    public String convert(String s, int numRows) {
        // First make a StringBuilder for each row.
        StringBuilder[] bufs = new StringBuilder[numRows];
        for (int i = 0; i < numRows; i++) {
            bufs[i] = new StringBuilder();
        }
        int i = 0;
        // Now an outer loop to exhaust the string.
        while (i < s.length()) {
            // The "zig" going down the rows.
            for (int j = 0; j < numRows && i < s.length(); j++) {
                bufs[j].append(s.charAt(i++));
            }
            // The "zag" going diagonal back up.
            for (int j = numRows - 2; j > 0 && i < s.length(); j--) {
                bufs[j].append(s.charAt(i++));
            }
        }
        // And then piece them all together.
        StringBuilder result = new StringBuilder();
        for (StringBuilder buf : bufs) {
            result.append(buf);
        }
        return result.toString();
    }
}
```
