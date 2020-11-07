# Multiply Strings

Given two non-negative integers `num1` and `num2` represented as strings,
return the product of `num1` and `num1` represented as a string.

**Note**: You must not use any built-in BigInteger library or convert the
inputs to integer directly.

## Hints

1. Use an array to accumulate the individual multiplications, and then
   use a second pass to do the digits and carries.

## Solutions

### Happy Array Day

This solution is O(len(num1) * len(num2)). It accumulates the multiplications
in their respective columns per usual multiplication rules and then does a
second pass to handle the carries and stripping to a single digit.

```java
class Solution {
    public String multiply(String num1, String num2) {
        int n1 = num1.length();
        int n2 = num2.length();
        int[] mult = new int[n1 + n2 + 1];
        for (int i = n1 - 1; i > -1; i--) {
            for (int j = n2 - 1; j > -1; j--) {
                // This adds the digits that are in the same "column".
                int val = (num1.charAt(i) - '0') * (num2.charAt(j) - '0');
                mult[mult.length - (n2 -j -1) - (n1 - i - 1) - 1] += val;
            }
        }
        // Now perform the carries and the strip down to one digit for each
        // column.
        for (int i = mult.length - 1; i > 0; i--) {
            mult[i - 1] += mult[i] / 10;
            mult[i] = mult[i] % 10;
        }
        // And finally build the string, not accumulating till we get passed
        // the leading zeroes.
        StringBuilder buf = new StringBuilder();
        boolean leading = true;
        for (int i = 0; i < mult.length; i++) {
            int val = mult[i];
            if (val == 0 && leading) {
                continue;
            }
            leading = false;
            buf.append(val);
        }
        return leading ? "0" : buf.toString();
    }
}
```
