# Number of Steps to Reduce a Number in Binary Representation to One

Given the binary representation of an integer as a string `s`, return the
number of steps to reduce it to `1` under the following rules:

*   If the current number is even, you have to divide it by `2`.
*   If the current number is odd, you have to add `1` to it.

It is guaranteed that you can always reach one for all test cases.

## Hints

1. The inputs are huge, think in terms of operating on the string itself and
   keeping the count.

## Solutions

### Iterating down the string

Given the size of the inputs converting it to an `int` or a `long` isn't
feasible. Perhaps `BigInteger` would work? Regardless, operating on the input
string itself is just fine. This approach works by doing just that. Here we
iterate from the least significant bit towards the most significant bit. At
each look we hit either a `0` or `1`. This indicates if we are even or odd
respectively. If we:

*   See `0` and have no carry, add one step which corresponds to a division
    because we are even. The shift would take us to the next bit.
*   See `0` and have a carry, add 2 steps. The value is `1` because of the
    carry. Thus we are looking at an odd and have to add `1` which results in
    the last bit becoming `0` and continues the carry. The last bit becoming
    `0` requires the division by 2 and shifts it off. Thus we add two steps,
    one to add the `1` and the second to divide by 2.
*   See `1` and have no carry, add 2 steps. We have to add `1` because we are
    looking at an odd. This makes the bit `0` and starts a carry. A division
    by 2 must be performed for this now even number which shifts us over to
    the next bit.
*   See `1` and have a carry, this means we actually have a `0` and continue
    the carry. To handle the `0` we must divide by 2, which shifts us to the
    next bit. Thus we add a single step.

We only iterate to the second to most significant bit. Since the most
significant bit is always a `1`, outside the loop we check if we have an active
carry. If we do we actually have `10`, and thus one more divide by 2 is
necessary.

```java
class Solution {
    public int numSteps(String s) {
        int steps = 0;
        boolean carry = false;
        for (int i = s.length() - 1; i > 0; i--) {
            if (s.charAt(i) == '0') {
                if (carry) {
                    steps += 2;
                } else {
                    steps++;
                }
            } else {
                if (carry) {
                    steps++;
                } else {
                    steps += 2;
                    carry = true;
                }
            }
        }
        if (carry) {
            steps++;
        }
        return steps;
    }
}
```
