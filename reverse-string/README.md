# Reverse String

Write a function that reverses a string. The input string is given as an array
of characters `char[]`.

Do not allocate extra space for another array, you must do this by **modifying
the input array in place** with O(1) extra memory.

You may assume all the characters consist of printable ascii characters.

## Hints

1. The original fizzbuzz?

## Solutions

### Loop de loop

Simple loop. Note that `mid` identifies the correct location to stop swapping
for both even and odd length strings.

```java
class Solution {
    public void reverseString(char[] s) {
        int last = s.length - 1;
        int mid = s.length / 2;
        for (int i = 0; i < mid; i++) {
            char temp = s[last - i];
            s[last - i] = s[i];
            s[i] = temp;
        }
    }
}
```
