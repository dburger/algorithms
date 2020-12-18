# Heaters

Winter is coming! During the contest, your first job is to design a standard
heater with a fixed warm radius to warm all the houses.

Every house can be warmed, as long as the house is within the heater's warm
radius range.

Given the positions of `houses` and `heaters` on a horizontal line, return
*the minimum radius standard of heaters so that those heaters could cover all
the houses.*

**Notice** that all the `heaters` follow your radius standard, and the warm
radius will be the same.

## Hints

1. For each home you could find the closest heater and expand r as necessary.

## Solutions

### Find the closes heater and expand r

This algorithm first sorts the heaters so that the closest heater can be looked
up efficiently. Then a loop over the houses finds the closest heater and expands
the radius r enough to cover that house.

The time complexity here would be:

O(n\*log(n)) + O(m\*log(n))

Where n is the length of `heaters` and m is the length of `houses`. The first bit
is from sorting `heaters`. The second bit is from the iteration over `houses` and
the associated lookup of the closest heater.

The trickiest part here is probably getting the `findClosest` binary search
correct.

```java
class Solution {
    public int findRadius(int[] houses, int[] heaters) {
        Arrays.sort(heaters);
        int r = 0;
        for (int i = 0; i < houses.length; i++) {
            int housePos = houses[i];
            int closestHeaterPos = findClosest(housePos, heaters);
            int diff = Math.abs(housePos - closestHeaterPos);
            if (diff > r) {
                r = diff;
            }
        }
        return r;
    }

    private int findClosest(int pos, int[] heaters) {
        int lo = 0;
        int hi = heaters.length - 1;
        while (lo <= hi) {
            int mid = lo + (hi - lo) / 2;
            int heaterPos = heaters[mid];
            if (heaterPos == pos) {
                return heaterPos;
            } else if (heaterPos < pos) {
                lo = mid + 1;
            } else {
                hi = mid - 1;
            }
        }

        // hi is less than lo, there are 3 cases to consider
        if (hi == -1) {
            return heaters[lo];
        } else if (lo == heaters.length) {
            return heaters[hi];
        } else {
            return pos - heaters[hi] < heaters[lo] - pos ? heaters[hi] : heaters[lo];
        }
    }
}
```
