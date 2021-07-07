# Count and Say

The **count-and-say** sequence is a sequence of digit strings defined by the
recursive formula:

*   `countAndSay(1) = "1"`
*   `countAndSay(n)` is the way you would "say" the digit string from
    `countAndSay(n - 1)`, which is then converted into the corresponding
    spoken digit string

To determine how you "say" a digit string, split it into the **minimal** number
of groups so that each group is a contiguous section all of the **same
character**. Then for each group, say the number of characters, then say the
character. To convert the saying into a digit string, replace the counts with
a number and concatenate every saying.

For each, the saying and conversion for digit string "3322251" is "23321511".
This is from "two 3s, three 2s, one 5, and one 1".

## Hints

1. The algorithm for conversion is fairly straightforard. How to advance
   through `n` iterations is up to you. Iterative or recursive should work.

## Solutions

### Iterate to your fate

This approach uses iteration through the `n` results to arrive at the final
count and say solution. It works with nested loops where the outer loop
keeps track of the `n` iterations and the inner loop handles an indivdual
iteration. The inner loop counts consecutive digits and accumlates them into
a `StringBuilder`. At the end of each inner loop `result` is set to this new
value.

```java
class Solution {
    public String countAndSay(int n) {
        String result = "1";
        while (n > 1) {
            int i = 0;
            StringBuilder sb = new StringBuilder();
            while (i < result.length()) {
                char ch = result.charAt(i);
                int count = 1;
                while (i < result.length() - 1 && result.charAt(i + 1) == ch) {
                    count++;
                    i++;
                }
                sb.append(count).append(ch);
                i++;
            }
            result = sb.toString();
            n--;
        }
        return result;
    }
}
```

### Recurse until you burst

The following solution translates the above solution into a recursive
solution. That is the decrement of `n` down to `1` is handled via recursive
calls rather than via a outer loop.

```java
class Solution {
    public String countAndSay(int n) {
        if (n == 1) {
            return "1";
        }

        String result = countAndSay(n - 1);

        StringBuilder sb = new StringBuilder();
        int i = 0;
        while (i < result.length()) {
            char ch = result.charAt(i);
            int count = 1;
            while (i < result.length() - 1 && result.charAt(i + 1) == ch) {
                count++;
                i++;
            }
            sb.append(count).append(ch);
            i++;
        }

        return sb.toString();
    }
}
```
