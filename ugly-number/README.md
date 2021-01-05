# Ugly Number

Write a program to check whether a given number is an "ugly number."

Ugly numbers are **positive numbers** whose prime factors only include
`2, 3, 5`.

## Hints

1. Keep dividing by the uglies until you are down to 1 or have determined
   "you can't get there from here." This could be done in an iterative or
   recursive fashion.

## Solutions

### Iterative till no change

This solution uses a loop to iterate until the number is no longer changing.
At that point, if we have `1` then we have an ugly number.

Note the usage of `0` for the `numBefore`. Initially I used a trick of setting
that to `-num`, however, the leeter instructions say that the range for the
input is `[-2^32, 2^31 - 1]`. Presumably if this are `Integer.MIN_VALUE` and
`Integer.MAX_VALUE` this would result in funny business if they passed
`Integer.MIN_VALUE`.

The time complexity here boils down to the main loop. In the worst case this
loop must take a number down to 1. Say that the only divisor used in taking it
down to 1 is the one that would do it the most slowly, the 2. The number would
be halved on each loop and thus the time complexity is O(log(n)). The space
complexity in this iterative version is O(1) as we only allocate a single
variable, `numBefore`, regardless of input size.

```java
class Solution {
    public boolean isUgly(int num) {
        if (num <= 0) return false;
        int numBefore = 0;
        while (num != numBefore) {
            numBefore = num;
            if (num % 5 == 0) num /= 5;
            if (num % 3 == 0) num /= 3;
            if (num % 2 == 0) num /= 2;
        }
        return num == 1;
    }
}
```

### Recursive turtles

This is just the iterative version translated into a recursive version. Note
that the time complexity remains the same O(log(n)) as the number will be at
least halved on each invocation. The story on the space complexity, however,
is different. Here the space complexity is O(log(n)) as the chain of recursive
calls can be O(log(n)) deep.

```java
class Solution {
    public boolean isUgly(int num) {
        if (num <= 0) return false;
        if (num == 1) return true;
        if (num % 5 == 0) {
            return isUgly(num / 5);
        } else if (num % 3 == 0) {
            return isUgly(num / 3);
        } else if (num % 2 == 0) {
            return isUgly(num / 2);
        } else {
            return false;
        }
    }
}
```
