# Reverse Vowels of a String

Given a string `s`, reverse only all the vowels in the string and return it.

## Hints

1. For speed, try one pass tracking for the next swap from head and tail.

## Solutions

### Two pass buffer and replay

Here we merely make two passes. The first pass stores the vowels in a separate
buffer. The second pass builds up the result and chooses from the buffer in
reverse when hitting a vowel.

The time complexity is O(n) to run through the string twice. The space
complexity is O(n) to hold the vowel list.

```java
class Solution {
    public String reverseVowels(String s) {
        StringBuilder vowels = new StringBuilder();
        for (char c : s.toCharArray()) {
            if (c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u' || c == 'A' || c == 'E' || c == 'I' || c == 'O' || c == 'U') {
                vowels.append(c);
            }
        }

        int i = vowels.length() - 1;
        StringBuilder result = new StringBuilder();
        for (char c : s.toCharArray()) {
            if (c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u' || c == 'A' || c == 'E' || c == 'I' || c == 'O' || c == 'U') {
                result.append(vowels.charAt(i--));
            } else {
                result.append(c);
            }
        }

        return result.toString();
    }
}
```

### One pass

Can we go faster? While we aren't going to beat the O(n) running time we can
make this go faster. Here we do that by doing one pass and having front and
tail pointers to quickly pick out the vowels to swap. That is `i` tracks the
front of the string and `j` tracks the tail of the string looking for the next
swap when advancing.

The time complexity of this solution is O(n). The space complexity is also
O(n), as we still create a `char[]` to build the solution into. In practice
this gets leeters highest 100.00% speed honor.

```java
class Solution {
    public String reverseVowels(String s) {
        char[] sarry = s.toCharArray();
        int i = 0;
        int j = sarry.length - 1;
        while (i < j) {
            char c = sarry[i];
            if (c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u' || c == 'A' || c == 'E' || c == 'I' || c == 'O' || c == 'U') {
                while (i < j) {
                    char d = sarry[j];
                    if (d == 'a' || d == 'e' || d == 'i' || d == 'o' || d == 'u' || d == 'A' || d == 'E' || d == 'I' || d == 'O' || d == 'U') {
                        sarry[i] = d;
                        sarry[j--] = c;
                        break;
                    }
                    j--;
                }
            }
            i++;
        }
        return new String(sarry);
    }
}
```
