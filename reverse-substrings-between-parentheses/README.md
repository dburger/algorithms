# Reverse Substrings Between Each Pair of Parentheses

You are given a string `s` that consists of lower case Engish letters and
brackets.

Reverse the strings in each pair of matching parentheses, starting from the
inntermost one.

Your result should not contain any brackets.

## Hints

1. Advance, identify parentheses pairs, and reverse within that range?

## Solutions

### Deque the halls

The follow solution attacks this problem with a `Deque`. The reason to use
a deque is that it makes it a little easier to think about this problem as
the string we be built out in order from start to end of the deque. The
solution works by iterating down the string, if we hit a `')'`, we remove
everything back to the matching `'('` and then add those characters back on
in reverse order. When we have iterated through the string the properly
reversed string will be resident in the deque.

Note that a tricky aspect of this problem is nested parentheses. When
parentheses are nested the innermost groups will be reversed multiple times.

```java
class Solution {
    public String reverseParentheses(String s) {
        Deque<Character> d = new LinkedList<>();
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            if (c == ')') {
                // Remove everything to the matching '(', storing the
                // removed characters in a list.
                char last = d.removeLast();
                List<Character> temp = new ArrayList<>();
                while (last != '(') {
                    temp.add(last);
                    last = d.removeLast();
                }
                // Now Add those characters back in reverse order.
                for (char e : temp) {
                    d.addLast(e);
                }
            } else {
                d.addLast(c);
            }
        }
        StringBuilder buf = new StringBuilder();
        while (!d.isEmpty()) {
            buf.append(d.removeFirst());
        }
        return buf.toString();
    }
}
```
