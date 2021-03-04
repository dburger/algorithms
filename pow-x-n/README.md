# Pow(x, n)

Implement `pow(x, n)`, which calculates `x` raised to the power `n`.

## Hints

1. Brute force just needs to reduce `n` while repeatedly multiplying by
   `x`. There is the negative `n` wrinkle, and perhaps some stack space
   concerns.
1. Can you adjust the idea from brute force to reduce the work? Think hard,
   think divide and conquer.

## Solutions

### Brute me recursively

Here we attempt to get by with the simplest brute force recursive solution.
Note the handling of a negative `n`. This will not pass the leeter because
the test set will have problems that lead to `StackOverflowError` with a
large `n`.

```java
class Solution {
    public double myPow(double x, int n) {
        if (n < 0) {
          return 1 / myPow(x, -n);
        }
        if (n == 0) {
          return 1.0;
        }
        return x * myPow(x, n - 1);
    }
}
```

### Brute me iteratively

Here, because of the `StackOverflowError` from above, we attempt to modify
to use iteration rather than recursion. While this problem evades the
`StackOverflowError` it doesn't evade leeter's timers. Here we get a time
limit exceeded.

```java
class Solution {
    public double myPow(double x, int n) {
        if (n < 0) {
          return 1 / myPow(x, -n);
        }
        double result = 1.0;
        while (n-- > 0) {
          result *= x;
        }
        return result;
    }
}
```

### Divide and conquer

Here, as alluded to in the second hint, we attempt to beat the time limit
exceeded by reducing the compuation needed. A common pattern in these power
problems is to use that fact that `pow(x, 2n) = pow(x, n) * pow(x, n)`. Thus
we can perform half the computation when we encounter an even power.

Note the wrinkle here for handling `n` with the value `Integer.MIN_VALUE`. In
this case `-Integer.MIN_VALUE` is no longer an `int`, thus the delegation
to a second method that handles `n` as a `long`.

```java
class Solution {
    public double myPow(double x, int n) {
        return mp(x, n);
    }

    // By translating the second parameter to a long, we
    // avoid a problem when n == Integer.MIN_VALUE since
    // -Integer.MIN_VALUE is not an Integer.
    public double mp(double x, long n) {
        if (n < 0) {
          return 1 / mp(x, -n);
        }
        if (n == 0) {
          return 1.0;
        }
        if (n % 2 == 0) {
          double result = mp(x, n / 2);
          return result * result;
        } else {
          return x * mp(x, n - 1);
        }
    }
}
```
