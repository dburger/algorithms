# Convert a Number to Hexadecimal

Given an integer `num`, return a string representing its hexadecimal
representation. For negative integers, two's complement method is used.

All the letters in the answer string should be lowercase characters, and there
should not be any leading zeros in the answer except for the zero itself.

**Note:** You are not allowed to use any built-in library methods to directly
solve this problem.

## Hints

1. This problem is mostly just a matter of being able to extract bits at
offsets from the input.

## Solutions

Note that the problem description states that -1 should be expressed as
"ffffffff" and not -1. That is this is a pure bit conversion with no negative
coming into play.

### Be shifty my friend

As noted in the hint the obvious solution is to extract hexadecimal sized bit
offset and translate those into the hexadecimal value. Below this is done
with the addition of a helper method.

The time complexity and space complexity of this solution is O(1). That is the
primary loop runs 8 or fewer times and the string bulider needs to hold at
most 8 characters.

```java
class Solution {
    private char extract(int val, int offset) {
        int h = (val >> (offset * 4)) & 0xf;
        if (h < 10) {
            return (char) ('0' + h);
        } else {
            return (char) ('a' + h - 10);
        }
    }

    public String toHex(int num) {
        StringBuilder result = new StringBuilder();
        for (int i = 7; i > -1; i--) {
            char c = extract(num, i);
            if ((c == '0' && result.length() > 0) || c != '0') {
                result.append(c);
            }
        }
        return result.length() > 0 ? result.toString() : "0";
    }
}
```
