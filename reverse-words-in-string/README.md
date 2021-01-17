# Reverse Words in a String

Given an input string `s`, reverse the order of the **words**.

A **word** is defined as a sequence of non-space characters. The **words** in
`s` will be separated by at least one space.

Return a string of the words in reverse order concatenated by a single space.

**Note** that `s` may contain leading or trailing spaces or multiple spaces
between two words. The returned string should only have a single space
separating the words. Do not include any extra spaces.

## Hints

1. Standard Java `split` should make this fairly easy, although it is likely to
   be just a bit slow on the leeter scale with all the `String` instantiation.
2. Like most Java string manipulation problems, you can probably get a speed
   boost by working with things as a character array.

## Solutions

### Using split

This is a standard split approach. Simple, works, and the time and space
complexity are O(n). Still, it is a little slow on the leeter scale.

```java
class Solution {
    public String reverseWords(String s) {
        String[] parts = s.split(" ");
        StringBuilder buf = new StringBuilder();
        for (int i = parts.length - 1; i > -1; i--) {
            String part = parts[i];
            if (!part.isBlank()) {
                buf.append(part).append(" ");
            }
        }
        if (buf.length() > 0) {
            buf.deleteCharAt(buf.length() - 1);
        }
        return buf.toString();
    }
}
```

### From a char array, squeezing speed

As noted in the hint above you often need to process strings via their
character array translations to squeeze a little speed out of these problems
in Java. The main speed increase here comes from avoiding `String`
instantiations. Here the `char[]` are appended into the `StringBuilder` and
the resultant `String` is constructed once at the very end. That is a `String`
is not made for each piece.

Here we iterate down the `char[]` in reverse looking for ending and then
starting points of the strings by looking for `' '`. The implementation is not
difficult (shouldn't this problem be leeter easy?) but the above slightly
simpler solution is probably good enough.

The time and space complexity for this implementation is also O(n) but in
practice it beats the above solution.

```java
class Solution {
    public String reverseWords(String s) {
        char[] schar = s.toCharArray();
        StringBuilder buf = new StringBuilder();
        int i = schar.length - 1;
        while (i > -1) {
            while (i > -1 && schar[i] == ' ') {
                i--;
            }
            if (i == -1) {
                break;
            }
            int anchor = i;
            while (i > -1 && schar[i] != ' ') {
                i--;
            }
            buf.append(schar, i + 1, anchor - i).append(' ');
        }
        if (buf.length() > 0) {
            buf.deleteCharAt(buf.length() - 1);
        }
        return buf.toString();
    }
}
```
