# Sum of Two Integers

Given two integers `a` and `b`, return the sum of the two integers without
using the operators `+` and `-`.

## Hints

1. Cover all combinations of `a < 0, a > 0` and `b < 0, b > 0`.

## Solutions

### Covering your bases

Here we merely cover all the combinations of positive and negative `a` and
`b` and use `++` and `--` accordingly.

The time complexity of this solution is O(max(abs(a, b))). That is we will
need to iterate a number of times equal to either `a` or `b` while adjusting
the other. Since we don't take care in this solution to choose the iteration
on the one with the smaller absolute value, the number of iterations could be
up to the larger absolute value. The space complexity of this algorithm is
O(1).

```java
class Solution {
    public int getSum(int a, int b) {
        if (a == 0) {
            return b;
        } else if (b == 0) {
            return a;
        } else if (a < 0 && b < 0) {
            while (b < 0) {
                b++;
                a--;
            }
            return a;
        } else if (a < 0 && b > 0) {
            while (a < 0) {
                a++;
                b--;
            }
            return b;
        } else if (a > 0 && b < 0) {
            while (b < 0) {
                b++;
                a--;
            }
            return a;
        } else {
            while (a > 0) {
                a--;
                b++;
            }
            return b;
        }
    }
}
```

### Covering your bases, tweaked

Here we use the same solution as above, but we take care to iterate per the
smaller absolute value. Thus the time complexity is changed to
O(min(abs(a, b))). The space complexity remains O(1).

```java
class Solution {
    public int getSum(int a, int b) {
        if (a == 0) {
            return b;
        }
        if (b == 0) {
            return a;
        }
        return Math.abs(a) < Math.abs(b) ? gs(a, b) : gs(b, a);
    }

    private int gs(int a, int b) {
        if (a < 0 && b < 0) {
            while (a < 0) {
                a++;
                b--;
            }
        } else if (a < 0 && b > 0) {
            while (a < 0) {
                a++;
                b--;
            }
        } else if (a > 0 && b < 0) {
            while (a > 0) {
                a--;
                b++;
            }
        } else {
            while (a > 0) {
                a--;
                b++;
            }
        }
        return b;
    }
}
```
