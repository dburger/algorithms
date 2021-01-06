# Ugly Number II

Write a program to find the `n`th ugly number.

Ugly numbers are **positive numbers** whose prime factors only include `2, 3, 5`.

Note that `1` is considered an ugly number.

## Hints

1. If you seed a sorted set, you can create something similar to a BFS solution
   by pulling from the set and then adding back in ugly numbers generated from
   prior ugly numbers.

## Solutions

### Generate and pull from sorted set

This is pretty simple solution with the hint in hand. The first ugly number `1`
is fed into a `TreeSet`. Then a loop begins that pulls out the smallest number
in that sorted set. If this is the `n`th ugly number it is returned. If not,
the ugly numbers that it would directly generate are inserted into the set,
and the loop continues.

```java
class Solution {
    public int nthUglyNumber(int n) {
        TreeSet<Long> t = new TreeSet<>();
        t.add(1L);
        for (;;) {
            long i = t.pollFirst();
            if (--n == 0) return (int) i;
            t.add(i * 2);
            t.add(i * 3);
            t.add(i * 5);
        }
    }
}
```
