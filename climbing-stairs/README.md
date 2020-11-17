# Climbing Stairs

You are climbing a staircase. It takes `n` steps to reach the top.

Each time you can either climb `1` or `2` steps. In how many distinct ways can
you climb to the to?

## Hints

1. Dynamic programming can work here. Build up an array that holds distinct
   ways and work your way to the top.

## Solutions

### Classic dynamic programming

```java
class Solution {
    public int climbStairs(int n) {
        if (n == 1) {
            return 1;
        }
        int[] stairs = new int[n];
        // One way to get to first step.
        stairs[0] = 1;
        // Two ways to get to second step, two one steps or one two step.
        stairs[1] = 2;
        for (int i = 2; i < n; i++) {
            // You have the ways that are a continuation of
            // 1) 1 step back
            // 2) 2 steps back
            stairs[i] = stairs[i - 1] + stairs[i - 2];
        }
        return stairs[n - 1];
    }
}
```
