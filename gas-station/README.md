# Gas Station

There are `N` gas stations along a circular route, where the amount of gas at
station `i` is `gas[i]`.

You have a car with an umlimited gas tank and it costs `cost[i]` of gas to
travel from station `i` to its next station `i + 1`. You begin the journey
with an empty tank at one of the gas stations.

Return the starting gas station's index if you can travel around the circuit
once in the clockwise direction, otherwise return -1.

## Hints

1. A brute force solution could just iterate from each starting position,
   seeing if it is possible to make the loop without going negative.
1. A better solution takes note of two things. First, the circuit is only
   possible if total gas is greater than total cost. Second, if it is possible,
   where to start? This is tricky. If it is possible, you can start from
   a starting point that never goes negative.

## Solutions

### Possible and never tank negative

This solution is O(n) where n is the number of gas stations.

```java
class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        int total = 0;
        int tank = 0;
        int start = 0;
        for (int i = 0; i < gas.length; i++) {
            total += gas[i] - cost[i];
            tank = tank + gas[i] - cost[i];
            if (tank < 0) {
                tank = 0;
                start = i + 1;
            }
        }
        return total >= 0 ? start : -1;
    }
}
```
