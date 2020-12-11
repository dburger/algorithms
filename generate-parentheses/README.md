# Generate Parentheses

Given `n` pairs of parentheses, write a function to generate all combinations
of well-formed parentheses.

## Hints

1. Recursion with an accumulator can do the trick, but how to proceed? Think
   in terms of how you check if parentheses are well formed. That is you proceed
   from left to right with an open parentheses count and verify that you never
   go negative and end up at 0. In this case you work to add parentheses only
   if they can lead to a valid solution (never go negative and end up at 0).

## Solutions

### Magic recursion

This algorithm is really elegant and simple to program. It follows the idea from
the hint above.

```java
class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> acc = new ArrayList<>();
        gp("", n, 0, 0, acc);
        return acc;
    }

    private void gp(String parens, int n, int open, int closed, List<String> acc) {
        if (open + closed == 2 * n) {
            // We have a full generation, add and return.
            acc.add(parens);
            return;
        }

        if (open < n) {
            // More open parens are valid, add one and recurse.
            gp(parens + "(", n, open + 1, closed, acc);
        }
        if (closed < open) {
            // Adding a closed would be valid here, add one and recurse.
            gp(parens + ")", n, open, closed + 1, acc);
        }
    }
}
```
