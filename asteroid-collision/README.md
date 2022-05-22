# Asteroid Collision

We are given an array `asteroids` of integers representing asteroids in a row.

For each asteroid, the absolute value represents its size, and the sign
represents its direction (positive meaning right, negative meaning left). Each
asteroid moves at the same speed.

Find out the state of the asteroids after all collisions. If two asteroids
meet, the smaller one will explode. If both are the same size, both will
explode. Two asteroids moving in the same direction will never meet.

## Hints

1. Classic problem where a newly encountered item can affect the items before
   it as it interacts with the nearest neighbor first. What data structure can
   exploit this?

## Solutions

### Pu, pu, pu, push it, on the stack

This problem is a problem ideally suited for using a stack. You are always
comparing the last two items, or walking back down the stack. It works by
placing items on the stack, popping the last two, and seeing if any of the
asteroids should be discarded.

The time complexity of this solution is O(n) where `n` is the size of the
input `asteroids`. This comes from iterating the elements up to three times.
Once to put them on the stack, once to remove them from the stack, and once
to produce the result. The space complexity of this solution is also O(n) as
the stack must potentially need to hold all of `asteroids`.

```java
class Solution {
    public int[] asteroidCollision(int[] asteroids) {
        Deque<Integer> d = new LinkedList<>();
        for (int a : asteroids) {
            d.addLast(a);
            while (d.size() > 1) {
                int right = d.removeLast();
                int left = d.removeLast();
                if (left > 0 && right < 0) {
                    int absleft = Math.abs(left);
                    int absright = Math.abs(right);
                    if (absleft == absright) {
                        // Both explode, not pushback, nothing left to check.
                        break;
                    } else {
                        int pushBack = absleft > absright ? left : right;
                        d.addLast(pushBack);
                    }
                } else {
                    // They don't interact with each other, push them back on. Nothing left to check.
                    d.addLast(left);
                    d.addLast(right);
                    break;
                }
            }
        }

        int i = 0;
        int[] result = new int[d.size()];
        while (!d.isEmpty()) {
            result[i++] = d.removeFirst();
        }

        return result;
    }
}
```
