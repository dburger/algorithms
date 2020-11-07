# Plus One

Given a **non-empty** array of decimal digits representing a non-negative
integer, increment one to the integer. The digits are stored such that the most
significant digit is at the head of the list, and each element in the array
contains a single digit.

You may assume the integer does not contain any leading zero except the number
0 itself.

## Hints

1. This can be handled with the typical add and carry approach in a loop.

## Solutions

### Add and carry loop

```java
class Solution {
    public int[] plusOne(int[] digits) {
        // Let's do this non-destructive to the input array.
        int[] copy = new int[digits.length];
        System.arraycopy(digits, 0, copy, 0, digits.length);
        int add = 1;
        int carry = 0;
        for (int i = copy.length - 1; i > -1; i--) {
            int val = copy[i] + carry + add;
            carry = val / 10;
            int digit = val % 10;
            copy[i] = digit;
            add = 0;
        }
        // Leftover carry, need another digit.
        if (carry > 0) {
            int[] temp = new int[copy.length + 1];
            System.arraycopy(copy, 0, temp, 1, copy.length);
            temp[0] = 1;
            copy = temp;
        }
        return copy;
    }
}
```
