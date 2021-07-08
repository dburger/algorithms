# Evaluate Reverse Polish Notation

Evaluate the value of an arithmetic expression in
[Reverse Polish Notation](http://en.wikipedia.org/wiki/Reverse_Polish_notation).

Valid operators are `+`, `-`, `*`, and `/`. Each operand may be an integer or
another expression.

**Note** that division between two integers should truncate toward zero.

It is guaranteed that the given RPN expression is always valid. That means the
expression would always evaluate to a result, and there will not be any
division by zero operation.

## Hints

1. As far as I know there is only one sane way to do this. This is one of the
   canonical examples of using a stack data structure. Most computer science
   students should have been exposed to this problem.

## Solutions

### Stack it

As noted in the hint this can be solved using a stack. The algorithm is simple.
Iterate through the inputs from left to right. When an operand (integer)
is encountered, push it on the stack. When an operator is encountered, pop the
top two items off the stack, operate on them, and push the rresult back on the
stack. The result will be the singular item on the stack at the end of this
processing.

Note the need to pull `v2` first and then `v1` off the stack. This is because
division is not commutative and this gives the correct order.

The time complexity and space complexity of this algorithm is O(n) where n is
the length of the input `tokens`. The comes from needing to iterate over every
token and the bound on the size of the stack for processing.

```java
class Solution {
    public int evalRPN(String[] tokens) {
        Deque<Integer> d = new LinkedList<>();
        for (int i = 0; i < tokens.length; i++) {
            String token = tokens[i];
            if (token.equals("+") || token.equals("-") || token.equals("*") || token.equals("/")) {
                Integer v2 = d.pop();
                Integer v1 = d.pop();
                if (token.equals("+")) {
                    d.push(v1 + v2);
                } else if (token.equals("-")) {
                    d.push(v1 - v2);
                } else if (token.equals("*")) {
                    d.push(v1 * v2);
                } else if (token.equals("/")) {
                    d.push(v1 / v2);
                }
            } else {
                d.push(Integer.valueOf(token));
            }
        }
        return d.pop();
    }
}
```
