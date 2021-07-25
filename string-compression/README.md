# String Compression

Given a array of characters `chars`, compress it using the following algorithm:

Begin with an empty string `s`. For each group of **consecutive repeating
characters** in `chars`:

*   If the group's length is 1, append the character to `s`.
*   Otherwise, append the character followed by the group's length.

The compressed string `s` **should not be returned separately**, but instead
stored **in the input character array** `chars`. Note that group lengths that
are 10 or longer will be split into multiple characters in `chars`.

After you are done **modifying the input array**, return *the new length of the
array*.

You must write an algorithm that uses only constant extra space.

## Hints

1. This is another variant on what I term "fill" tracking algorithms. That is
   you keep track of a `fill` position and fill into it.

## Solutions

### Fill it like you mean it

Here we use the `fill` technique to simply solve this problem. The trick to
understand is that you are always consuming at least as much as you are
filling, and thus you don't end up overwriting data before you use it.

The time complexity of this algorithm is O(n) as only a single pass through
the data is needed. The space complexity is O(1) as the algorithm fills into
the input array.

```java
class Solution {
    public int compress(char[] chars) {
        int fill = 0;
        char last = chars[0];
        int count = 1;
        for (int i = 1; i < chars.length; i++) {
            char next = chars[i];
            if (next == last) {
                count++;
            } else {
                chars[fill++] = last;
                fill = fillDigits(chars, fill, count);
                count = 1;
            }
            last = next;
        }
        chars[fill++] = last;
        return fillDigits(chars, fill, count);
    }

    private int fillDigits(char[] chars, int fill, int count) {
        if (count == 1) {
            // Nothing to fill, 1 doesn't get a digit.
        } else if (count < 10) {
            // Optimization, avoid making the String.
            chars[fill++] = Character.forDigit(count, 10);
        } else {
            for (char c : String.valueOf(count).toCharArray()) {
                chars[fill++] = c;
            }
        }
        return fill;
    }
}
```
