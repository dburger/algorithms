# Excel Sheet Column Number

Given a string `columnTitle` that represents the column title as it appears
in an Excel sheet, return its corresponding column number.

For example:

```
A  ->  1
B  ->  2
C  ->  3
...
Z  -> 26
AA -> 27
AB -> 28
```

## Hints

1. Positional expansion no?

## Solutions

### Straight forward conversion

The obvious answer works and is fast. Proceed by expanding each character
into its positional value and sum them all together.

Ignoring the fact that this problem is limited to what can be returned in an
int, the time complexity is O(number of characters in the input). The space
complexity is O(1).

```java
class Solution {
    public int titleToNumber(String columnTitle) {
        int digits = columnTitle.length();
        int result = 0;
        for (int i = 0; i < columnTitle.length(); i++) {
            int num = columnTitle.charAt(i) - 'A' + 1;
            int val = (int) Math.pow(26, digits - i - 1);
            result += num * val;
        }
        return result;
    }
}
```
