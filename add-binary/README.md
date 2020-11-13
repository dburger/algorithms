# Add Binary

Given two binary strings, return their sum (also a binary string).

## Hints

1. Obviously translations to ints and then adding and translating back
   to a binary string would be pretty trivial. The intention is probably
   to keep these largely as strings and to "add and carry."

## Solutions

### Add and carry

```java
class Solution {
    public String addBinary(String a, String b) {
        int aEnd = a.length() - 1;
        int bEnd = b.length() - 1;
        boolean carry = false;
        StringBuilder buf = new StringBuilder();
        // String result = "";
        while (aEnd > -1 || bEnd > -1) {
            char aChar = (aEnd > -1 ? a.charAt(aEnd) : '0');
            char bChar = (bEnd > -1 ? b.charAt(bEnd) : '0');
            if (aChar == '0' && bChar == '0') {
                buf.append(carry ? "1" : "0");
                // result = (carry ? "1" : "0") + result;
                carry = false;
            } else if (aChar == '1' && bChar == '1') {
                buf.append(carry ? "1" : "0");
                // result = (carry ? "1" : "0") + result;
                carry = true;
            } else {
                buf.append(carry ? "0" : "1");
                // result = (carry ? "0" : "1") + result;
            }
            aEnd--;
            bEnd--;
        }
        // return carry ? "1" + result : result;
        if (carry) {
            buf.append("1");
        }
        return buf.reverse().toString();
    }
}
```
