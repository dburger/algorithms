# Happy Number

Write an algorithm to determine if a number `n` is happy.

A **happy number** is a number defined by the following process:

*   Starting with any positive integer, replace the number by the sum of
    squares of its digits.
*   Repeat the process until the number equals 1 (where it will stay), or it
    **loop endlessly in a cycle** which does not include 1.
*   Those numbers for which this process **ends in 1** are happy.

Return `true` if `n` is a happy number, and `false` if it is not.

## Hints

1. Brute works, but there are better brutes.

## Solutions

### Brute take one

The first brute force approach just takes the most obvious path. Write the
corresponding calculating algorithm and use a `Set` to detect a cycle.

```java
class Solution {
    public boolean isHappy(int n) {
        Set<Integer> seen = new HashSet<>();
        seen.add(n);
        while (n != 1) {
            n = sos(n);
            if (!seen.add(n)) {
                return false;
            }
        }
        return true;
    }

    private int sos(int n) {
        int sum = 0;
        while (n > 0) {
            int digit = n % 10;
            n /= 10;
            sum += digit * digit;
        }
        return sum;
    }
}
```

### Brute take two

The brute force approach above works. We can squeeze a bit more performance out
of it by avoiding the `Set` and the creation of boxed `Integer`s that go into
it. How so? Well, looking closer the determining if there is a cycle is the
same as determining if there is a cycle in a linked list. This can be done by
using "fast" and "slow" pointers and seeing if they intersect or if fast
reaches the end of the list. In this case, that becomes fast reducing to `1`
before they intersect.

```java
class Solution {
    public boolean isHappy(int n) {
        int slow = sos(n);
        int fast = sos(slow);
        while (fast != 1 && fast != slow) {
            slow = sos(slow);
            fast = sos(sos(fast));
        }
        return fast == 1;
    }

    private int sos(int n) {
        int sum = 0;
        while (n > 0) {
            int digit = n % 10;
            n /= 10;
            sum += digit * digit;
        }
        return sum;
    }
}
```
