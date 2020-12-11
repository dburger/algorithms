# Generate Parentheses

Given `n` pairs of parentheses, write a function to generate all combinations
of well-formed parentheses.

## Hints

1. Brute force - generate all possible strings and add them if they are valid.
   You know how to check for valid right? Proceed from left to right and keep
   an open parens count. If never goes negative and finishes at 0 it is valid.
1. Recursion with an accumulator can do the trick, but how to proceed? Think
   in terms of how you check if parentheses are well formed (as mentioned in
   prior hint). In this case you work to add parentheses only if they can lead
   to a valid solution.

## Solutions

### Brute force

Here we use brute force to generate all possible parentheses strings and only
add them if they are valid.

```java
class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> acc = new ArrayList<>();
        gp("", 2 * n, acc);
        return acc;
    }

    private void gp(String parens, int len, List<String> acc) {
        if (len == 0) {
            if (valid(parens)) {
                acc.add(parens);
            }
            return;
        }

        gp(parens + "(", len - 1, acc);
        gp(parens + ")", len - 1, acc);
    }

    private boolean valid(String parens) {
        int open = 0;
        for (int i = 0; i < parens.length(); i++) {
            switch (parens.charAt(i)) {
              case '(':
                  open++;
                  break;
              default:
                  open--;
            }
            if (open < 0) {
                return false;
            }
        }
        return open == 0;
    }
}
```

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
