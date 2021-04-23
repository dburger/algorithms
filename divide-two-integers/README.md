# Divide Two Integers

Given two integers `dividend` and `divisor`, divide two integers without using
mutliplication, division, and mod operator.

Return the quotient after dividing `dividend` by `divisor`.

The integer division should truncate toward zero, which means losing its
fractional part. For example, `truncate(8.345) = 8` and
`truncate(-2.7335) = -2`.

**Note:** Assume we are dealing with an environment that could only store
integers within the **32-bit** signed integer range:
`[-2^31, 2^31 - 1]`. For this problem assume that your function returns
`2^31 - 1` when the division result overflows.

## Hints

1. Obviously division is repeated subtraction. That should give you a brute
   force approach.
1. The **Note** above places constraints that make this problem a pain. That
   is you could just handle the values internally as longs, to avoid the
   overflow problem a bit, but that is disallowed by the note.
1. If you can't multiply, divide, or mod, is there another mathematical
   operation that can help you?

## Solutions

### Cheating brute

Here we give the cheating, because we internally use longs as forbidden in
the note, brute force approach. That is we merely use repeated subtraction and
count the number of times we can fully subtract the `divisor` from the
`dividend`.

This approach does not pass the leeter's time limits. The time complexity
of this approach is O(n). That is there can be a loop that iterates the size of
`dividend`, as each loop may only remove `1` at a time if this is the value of
`divisor`. The space complexity of this solution is O(1).

```java
public int divide(int dividend, int divisor) {
    boolean negative = dividend < 0 ^ divisor < 0;
    long numerator = Math.abs((long) dividend);
    long denominator = Math.abs((long) divisor);
    long count = 0;
    while (numerator >= denominator) {
        numerator -= denominator;
        count++;
    }
    if (negative) {
        count = -count;
    }
    return count < Integer.MIN_VALUE || count > Integer.MAX_VALUE ? Integer.MAX_VALUE : (int) count;
}
```

### You shifty bastard

We can't use multiply, divide, or mod, but we can use shifts. Recall that
shifting to the left by one bit is equivalent to multiplying by 2. Thus instead
of reducing by `divisor` in each iteration of the loop, we can reduce by the
largest power of 2 multiple of the `divisor` on each iteration.

The overflow problem still remains. Since the inputs can be
`Integer.MIN_VALUE`, taking that to a positive is an overflow as
`Integer.MIN_VALUE = -2^31` and `Integer.MAX_VALUE = 2^31 - 1`. To avoid that
problem we somewhat unnaturally work with the `dividend` and `divisor` as their
negatives here.

```java
class Solution {
    public int divide(int dividend, int divisor) {
        if (dividend == Integer.MIN_VALUE && divisor == -1) {
            // The only overflow case, -Integer.MIN_VALUE > Integer.MAX_VALUE
            return Integer.MAX_VALUE;
        }
        boolean negative = dividend < 0 ^ divisor < 0;
        // We'll work with negatives since -Integer.MAX_VALUE is still valid
        // while -Integer.MIN_VALUE is not.
        if (dividend > 0) {
            dividend = -dividend;
        }
        if (divisor > 0) {
            divisor = -divisor;
        }
        int count = 0;
        while (dividend <= divisor) {
            int shift = 0;
            int d = divisor << shift;
            while (dividend <= d) {
                shift++;
                int before = d;
                d = divisor << shift;
                // Determine if the shift caused an overflow.
                if (d > before) {
                    break;
                }
            }
            shift--;
            dividend -= divisor << shift;
            count += 1 << shift;
        }
        return negative ? -count : count;
    }
}
```
