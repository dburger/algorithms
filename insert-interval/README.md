# Insert Interval

Given a set of *non-overlapping* intervals, insert a new interval into the
intervals (merge if necessary).

You may assume that the intervals were initially sorted according to their
start times.

## Hints

1. This can be done with a fairly simple iteration on whether the new interval
   is before, overlaps with, or is after a current interval. The twist is to
   keep expanding the new interval until it is ready to insert.

## Solutions

### Simple loopage

```java
class Solution {
    public int[][] insert(int[][] intervals, int[] newInterval) {
        // Make an array to hold the result. If there are no merges
        // it could be one larger than the original.
        int[][] result = new int[intervals.length + 1][2];
        int fill = 0;
        boolean consumed = false;
        for (int i = 0; i < intervals.length; i++) {
            if (before(newInterval, intervals[i])) {
                // We have found the insertion point, insert and then
                // insert the remaining.
                result[fill++] = newInterval;
                int remaining = intervals.length - i;
                System.arraycopy(intervals, i, result, fill, remaining);
                fill += remaining;
                consumed = true;
                break;
            } else if (overlap(newInterval, intervals[i])) {
                // We are overlapped with current, expand newInterval.
                newInterval = merge(newInterval, intervals[i]);
            } else {
                // We are after, just add the current interval.
                result[fill++] = intervals[i];
            }
        }

        // If we were never consumed, then newInterval is the last element.
        if (!consumed) {
            result[fill++] = newInterval;
        }

        // See if we are shorter than result, and if so, truncate back to
        // the correct size.
        if (fill < result.length) {
            int[][] temp = new int[fill][2];
            System.arraycopy(result, 0, temp, 0, fill);
            result = temp;
        }

        return result;
    }

    private boolean before(int[] a, int[] b) {
        return a[1] < b[0];
    }

    private boolean overlap(int[] a, int[] b) {
        return a[0] <= b[1] && a[1] >= b[0];
    }

    private int[] merge(int[] a, int[] b) {
        return new int[] {Math.min(a[0], b[0]), Math.max(a[1], b[1])};
    }
}
```
