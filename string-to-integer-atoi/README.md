# String to Integer (atoi)

Implement `atoi` which converts a string to an integer.

The function first discards as many whitespace characters as necessary until the
first non-whitespace character is found. Then, starting from this character
takes an optional initial plus or minus sign followed by as many numerical
digits as possible, and interprets them as a numerical value.

The string can contain additional characters after those that form the integral
number, which are ignored and have no effect on the behavior of this function.

If the first sequence of non-whitespace characters in the str is not a valid
integral number, or if no such sequence exists because either str is empty or
it contains only whitespace characters, no conversion is performed.

If no valid conversion could be performed, a zero value is returned.

Note:

* Only the space character ` ` is considered a whitspace character.
* Assume we are dealing with an environment that could only store integers
  within the 32-bit signed integer range: [-2^32, 2^31 - 1]. If the numerical
  value is out of the rantge of representable values, INT_MAX (2^31 - 1) or
  INT_MIN (-2^31) is returned.

## Hints

1. The devil is in the details here. Pay close attention from the verbiage above
   to what should be converted and what should be a throw away scenario. Also
   adding to the pain, is the overflow conditions stated in the note above.

## Solutions

### Brute force

```java
class Solution {
    public int myAtoi(String s) {
        int i = 0;
        // Consume any leading whitespace
        for (; i < s.length(); i++) {
            if (s.charAt(i) != ' ') {
                break;
            }
        }

        if (i == s.length()) {
            // Empty or only whitespace.
            return 0;
        }

        // Consume sign, if any.
        boolean negative = false;
        if (s.charAt(i) == '-') {
            negative = true;
            i++;
        } else if (s.charAt(i) == '+') {
            i++;
        }

        long value = 0;
        while (i < s.length()) {
            char c = s.charAt(i++);
            int cval = c - '0';
            if (cval < 0 || cval > 9) {
                // Non valid character after valid, stop accumulating.
                break;
            }
            long priorValue = value;
            value = cval + (value * 10);
            if (value > Integer.MAX_VALUE) {
                // We are overflowing out int.
                return negative ? Integer.MIN_VALUE : Integer.MAX_VALUE;
            }
        }

        return negative ? (int) -value : (int) value;
    }
}
```
