# Valid Parentheses

Given a string `s` containing just the characters `(`, `)`, `{`, `}`,
`[`, and `]`, determine if the input string is valid.

An input string is valid if:

1. Open brackets must be close by the same type of bracket.
1. Open brackets must be closed in the correct order.

## Hints

1. Classic stack problem.

## Solutions

### Stack em up and match em up

Here we use the classic stack approach for matching the various bracket
types. When we see an opener we push it. When we see a closer we make
sure the top of the stack contains the correct opener. At the end if the
stack is empty, the everything was consumed correctly.

The time complexity of this algorithm is O(n) where n is the length of the
input string. The space complexity is also O(n), to accomodate the processing
of the string in the stack.

```java
class Solution {
    public boolean isValid(String s) {
        Deque<Character> d = new LinkedList<>();
        for (Character c : s.toCharArray()) {
            if (c == '(' || c == '{' || c == '[') {
                d.push(c);
            } else {
                // Was a closer, top must match.
                Character top = d.pollFirst();
                if (top == null) {
                    return false;
                } else if (c == ')' && top != '(') {
                    return false;
                } else if (c == '}' && top != '{') {
                    return false;
                } else if (c == ']' && top != '[') {
                    return false;
                }
            }
        }
        return d.isEmpty();
    }
}
```
