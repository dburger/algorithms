# Reverse Integer

Given a signed 32-bit integer `x`, return `x` with its digits reversed. If
reversing `x` causes the value to go outside the signed 32-bit integer range
`[-2^31, 2^31 - 1]`, then return `0`.

**Assume the environment does not allow you to store 64-bit integers (signed
or unsigned).**

While this is an "easy" problem on the leeter, I very much like this problem.
In it are some building blocks that are used in a lot of different problems,
they include:

1. Peeling of digits one at a time.
1. Horner's method to arrive at a number's value folding in one digit at a time.
1. Avoiding overflow.

The last one can be done in several different ways. I present some of them
below.

## Hints

1. You could obviously use `String.valueOf(x)` to get a string and access the
   digits one by one here, but it is more efficient and considered better style
   to peel off digits with the mod function. Note that the solutions below could
   be adjusted to work with a `String.valueOf(x)` approach.
1. Horner's method is nice.
1. The obvious wrinkle in this problem is that it can overflow. How are you
   going to deal with that?

## Solutions

### Horner's method basic, avoiding pitfalls and constraints

This is a basic solution for the problem that completely avoids some of the
pitfalls and constraints that are specified about. Notably, this solution does
not handle the overflow situation. Because of this, this solution will not
pass the leeter grader.

```java
class Solution {
    public int reverse(int x) {
        boolean negative = x < 0;
        if (negative) {
            x = -x;
        }
        int result = 0;
        while (x > 0) {
            int digit = x % 10;
            x /= 10;
            result = result * 10 + digit
        }
        return negative ? -result : result;
    }
}
```

### Horner's method protected with long

Here we avoid the overflow problem and ignore the assumption posited in the
problem description and merely use a long to store the computed value. Note
that the problem description forbids this approach, with its "Assume the
environment does not allow you to store 64-bit integers" blurb.

```java
class Solution {
    public int reverse(int x) {
        boolean negative = x < 0;
        if (negative) {
            x = -x;
        }
        long result = 0;
        while (x > 0) {
            int digit = x % 10;
            x /= 10;
            result = result * 10 + digit;
        }
        if (result > Integer.MAX_VALUE) {
            return 0;
        }
        return negative ? (int) -result : (int) result;
    }
}
```

### Horner's method protected with Java's `Math.*exact`

Here we use some java built in functions to detect the overflow.

```java
class Solution {
    public int reverse(int x) {
        boolean negative = x < 0;
        if (negative) {
            x = -x;
        }
        int result = 0;
        while (x > 0) {
            int digit = x % 10;
            x /= 10;
            try {
                // These methods throw ArithmeticException on overflow.
                // result = result * 10 + digit;
                result = Math.multiplyExact(result, 10);
                result = Math.addExact(result, digit);
            } catch (ArithmeticException e) {
                return 0;
            }
        }
        return negative ? -result : result;
    }
}
```

### Horner's method protected with boundary check

Finally, we just make everything explicit. Since each iteration of the loop
using Horner's method to fold in another digit does a:

* multiple previous result times 10
* add in the new digit

then we know that we will overflow if the previous result is:

* larger than `Integer.MAX_VALUE / 10`, because the `* 10` will take it over
* if equal to `Integer.MAX_VALUE / 10`, if the new digit is larger than 7, since
  `Integer.MAX_VALUE == 2147483647` and thus `* 10 + digit` will take it over

```java
class Solution {
    public int reverse(int x) {
        int boundary = Integer.MAX_VALUE / 10;
        boolean negative = x < 0;
        if (negative) {
            x = -x;
        }
        int result = 0;
        while (x > 0) {
            int digit = x % 10;
            x /= 10;
            if (result > boundary || (result == boundary && digit > 7)) {
                return 0;
            }
            result = result * 10 + digit;
        }
        return negative ? -result : result;
    }
}
```
