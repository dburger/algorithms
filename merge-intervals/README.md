# Merge Intervals

Given an array of `intervals` where `intervals[i] = [starti, endi]`,
merge all overlapping intervals and return an array of the non-overlapping
intervals that cover all the intervals in the input.

## Hints

1. The real trick here it to start by getting the intervals that can
   possibly merged are in contiguous runs. From there you should be able
   to track the prior and see if the next can merge with it.

## Solutions

### Sort and iterate

Here we start by sorting the input arrays so they are in an order conducive
for merging. That is, arrays that could possibly merged are placed in contiguous
runs. With that done, we keep track of the prior interval and check if the
current interval can merge with it. If it can, that is what we do. If not, we
add the prior and then make curr prior.

The time complexity of this algorithm is dominated by the sort of the arrays
making it O(n\*log(n)). The space complexity is O(n) as the result array may
need an entry for each input entry.

```java
class Solution {
    public int[][] merge(int[][] intervals) {
        Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
        List<int[]> merged = new ArrayList<>();
        int[] prior = intervals[0];
        for (int i = 1; i < intervals.length; i++) {
            int[] curr = intervals[i];
            if (overlaps(prior, curr)) {
                prior = merged(prior, curr);
            } else {
                merged.add(prior);
                prior = curr;
            }
        }
        merged.add(prior);
        int[][] result = new int[merged.size()][2];
        for (int i = 0; i < merged.size(); i++) {
            int[] val = merged.get(i);
            result[i][0] = val[0];
            result[i][1] = val[1];
        }
        return result;
    }

    private boolean overlaps(int[] a, int[] b) {
        return b[0] <= a[1] && b[1] >= a[0];
    }

    private int[] merged(int[] a, int[] b) {
        int[] result = new int[2];
        result[0] = Math.min(a[0], b[0]);
        result[1] = Math.max(a[1], b[1]);
        return result;
    }
}
```
