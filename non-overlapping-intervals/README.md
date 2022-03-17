# Non-overlapping Intervals

Given an array of intervals `intervals` where `intervals[i] = [starti, endi]`,
return the minimum number of intervals you need to remove to make the rest of
the intervals non-overlapping.

## Hints

1. Sort but how?

## Solutions

### Sort by ending time

If you sort by the ending time then we can iterate through the intervals
keeping `prev` and `curr` indicators, throwing away any interval that overlaps
with the previous interval.

The time complexity of this algorithm is dominated by the initial sort which
is O(n * log(n)) where n is the number of intervals. The space complexity of
this solution is O(1).

```java
class Solution {
    public int eraseOverlapIntervals(int[][] intervals) {
        Arrays.sort(intervals, (a, b) -> a[1] - b[1]);
        int count = 0;
        int prev = 0;
        int curr = 1;
        while (curr < intervals.length) {
            if (intervals[curr][0] < intervals[prev][1]) {
                // curr overlaps, throw it away
                count++;
            } else {
                // curr doesn't overlap, keep it and consider the next
                prev = curr;
            }
            curr++;
        }
        return count;
    }
}
```

### Sort by starting time

How does the problem change if we sort by the starting time instead? We do the
same `prev` and `curr` comparison as before. In the comparison, however, we
accept `prev` if there is no overlap. If there is an overlap we keep the one
that ends first and throw the other away.

The time and space complexity of this solution is the same as the prior
solution. That is O(n * log(n)) time complexity, and O(1) space complexity.

```java
class Solution {
    public int eraseOverlapIntervals(int[][] intervals) {
        Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
        int count = 0;
        int prev = 0;
        int curr = 1;
        while (curr < intervals.length) {
            if (intervals[curr][0] >= intervals[prev][1]) {
                // No overlap, accept prev and advance.
                prev = curr;
            } else {
                // Overlap, keep the one that ends first and throw the other away.
                count++;
                if (intervals[curr][1] < intervals[prev][1]) {
                    prev = curr;
                }
            }
            curr++;
        }
        return count;
    }
}
```
