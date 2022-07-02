# Distance Between Bus Stops

A bus has `n` stops numbered from `0` to `n - 1` that form a circle. We know the
distance between all pairs of neighboring stops where `distance[i]` is the
distance between the stops `i` and `(i + 1) % n`.

The bus goes along both directions i.e. clockwise and counterclockwise.

Return the shortest distance between the given `start` and `destination` stops.

## Hints

1. Hey, this is an easy.

## Solutions

### Count da one, subtract for the other

This solution uses modular math to count around the array from `start` to
`dest` to get the distance in the "natural" forward direction. The distance
for the opposite direction is determined by subtracting this value from the
total.

The time complexity of this solution is O(n) where `n` is the number of
elements in `distances`. The space complexity of this solution is O(1).

```java
class Solution {
    public int distanceBetweenBusStops(int[] distances, int start, int dest) {
        int total = 0;
        for (int d : distances) {
            total += d;
        }

        int forward = 0;
        int i = start;
        while (i != dest) {
            forward += distances[i];
            i = (i + 1) % distances.length;
        }

        return Math.min(forward, total - forward);
    }
}
```

### Let's optimize unnecessarily, it will be fun

OK, well the leet grader with its percentile score is a little addicting. While
we can't beat the big O time complexity of the solution above, we can take one
pass through the input rather than two. We do this by seperating into partial
loops that can't the "forward" and "reverse" directions.

As noted above the time complexity remains O(n). The space complexity is also
O(1).

```java
class Solution {
    public int distanceBetweenBusStops(int[] distances, int start, int dest) {
        int forward = 0;
        int i = start;
        while (i != dest) {
            forward += distances[i];
            i = (i + 1) % distances.length;
        }

        int reverse = 0;
        i = dest;
        while (i != start) {
            reverse += distances[i];
            i = (i + 1) % distances.length;
        }

        return Math.min(forward, reverse);
    }
}
```
