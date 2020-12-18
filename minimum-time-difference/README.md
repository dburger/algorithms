# Minimum Time Difference

Given a list of 24-hour clock time points in **"HH:MM"** format, return
*the minimum **minutes** difference between any two time-points in the
list.*

## Hints

1. These times in minutes will easily fit in an array.
1. There is only one tricky part here. That is "00:00" is naturally at
   the start of an ordering while "23:59" is at the end. These, however,
   are considered one minute apart.

## Solutions

### Overlap

The tricky part mentioned in the hints above can be handled by doing a wrap
around in the array using a modulus. This time complexity of this algorithm
is O(n) due to the loop on `timePoints`...wait, or is it. See the next solution
for a rather simple extension.

```java
class Solution {
    public int findMinDifference(List<String> timePoints) {
        boolean[] slots = new boolean[1440];
        for (String tp : timePoints) {
            int hours = (tp.charAt(0) - '0') * 10 + (tp.charAt(1) - '0');
            int minutes = hours * 60 +
                (tp.charAt(3) - '0') * 10 + (tp.charAt(4) - '0');
            if (slots[minutes]) {
                return 0;
            }
            slots[minutes] = true;
        }
        int prior = -1;
        int minDiff = Integer.MAX_VALUE;
        // We wrap around to get values in the second half closer to
        // something in the first half. Note the modulus to map back
        // to a time slot.
        for (int i = 0; i < 2160; i++) {
            if (slots[i % 1440]) {
                if (prior > -1) {
                    minDiff = Math.min(minDiff, i - prior);
                }
                prior = i;
            }
        }
        return minDiff;
    }
}
```

### Overlap O(1) solution

Aha! Via the
[pigeonhole principle](https://en.wikipedia.org/wiki/Pigeonhole_principle)
the time complexity of this algorithm is O(1) with only a simple tweak
to the above code. That is, if there are more than 1440 time points then
there has to be a duplicate time point and the answer is 0. Thus there is
a bound on the major loop.

```java
class Solution {
    public int findMinDifference(List<String> timePoints) {
        if (timePoints.size() > 1440) {
            return 0;
        }
        boolean[] slots = new boolean[1440];
        for (String tp : timePoints) {
            int hours = (tp.charAt(0) - '0') * 10 + (tp.charAt(1) - '0');
            int minutes = hours * 60 +
                (tp.charAt(3) - '0') * 10 + (tp.charAt(4) - '0');
            if (slots[minutes]) {
                return 0;
            }
            slots[minutes] = true;
        }
        int prior = -1;
        int minDiff = Integer.MAX_VALUE;
        // We wrap around to get values in the second half closer to
        // something in the first half. Note the modulus to map back
        // to a time slot.
        for (int i = 0; i < 2160; i++) {
            if (slots[i % 1440]) {
                if (prior > -1) {
                    minDiff = Math.min(minDiff, i - prior);
                }
                prior = i;
            }
        }
        return minDiff;
    }
}
```
