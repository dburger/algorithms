# Factorial Trailing Zeroes

Given an integer `n`, return the number of trailing zeroes in `n!`.

**Follow up:** Could you write a solution that works in logarithmic time
complexity?

## Hints

1. A brute force solution could compute the factorial and then see how many
   times the result can be divided by 10 with no remainder.
1. How can you form a trailing zero in a factorial? What factors are involved?
   Can you compute an answer by considering the factors of the factorial in
   relation to the factors needed to form trailing zeroes?

## Solutions

### Brute force

This is the brute force solution. We merely compute the factorial, and then
see how many times we can divide by ten without a remainder. Note that this
solution won't pass the leeter's time limit! Also note that this solution
needed to use `BigInteger` to be able to compute the size of factorials
present in the test cases.

```java
import java.math.BigInteger;

class Solution {
    public int trailingZeroes(int n) {
        BigInteger value = BigInteger.ONE;
        while (n > 2) {
            value = value.multiply(BigInteger.valueOf(n--));
        }

        int count = 0;
        while (value.mod(BigInteger.TEN).equals(BigInteger.ZERO)) {
            value = value.divide(BigInteger.TEN);
            count++;
        }
        return count;
    }
}
```

### Counting fives

This solution goes down the trail of the second hint. In order for a factorial
to have a trailing zero it must contain a factor of 10. The prime factorization
of 10 is 2 and 5. Because 2 is a factor in all even numbers, there are many more
2 factors in a given factorial than 5 factors. Therefore the number of trailing
zeroes in a factorial is equivalent to the number of 5 factors in that
factorial. So how do you compute this?

Well first say we are computer `45!`. Within this number we have the following,
`{5, 10, 15, 20, 25, 30, 35, 40, 45`, that contain a `5` factor. It is pretty
clear that a first step is taking `n` and dividing by `5`. In this case we get
`9`. This is not correct, however. Note that the `25` in that last has two `5`
factors. This leads to the correct solution that the number of trailing zeroes
is equal to the number of times `{5, 25, 125, ...}`, that is the powers of `5`,
divide into `n`.

This can be done in a slick way. Consider the `25`. `n / 25 == n / 5 / 5`. Thus
the counts for the different powers of `5` can be done in a handy loop.

```java
class Solution {
    public int trailingZeroes(int n) {
        int count = 0;
        while (n > 4) {
            n /= 5;
            count += n;
        }
        return count;
    }
}
```
