# Find Right Interval

You are given an array of `intervals`, where `intervals[i] = [start, end]` and
each `start` is **unique**.

The **right interval** for an interval `i` is an interval `j` such that
`start_j >= end_i` and `start_j` is **minimized**. Note that `i` may equal `j`.

Return an array of **right interval** indices for each interval `i`. If not
**right interval** exists for interval `i`, then put `-1` at index `i`.

## Hints

1. If we sort the intervals by the first value, along with bookkeeping to keep
   track of the original index, can we efficiently use this structure to find
   first start >= end?
2. If we produce two sorted lists, one by inverval first values and one by
   interval second values, can we then solve with single passes through these
   lists?

## Solutions

### Sort the list, binary search for joy

This solution first produces a sorted version of the list with an original
index indicator. The list is sorted per the first element of the interval.
After this sorting is complete a pass is made through the list with an
accompanying binary search to locate the correct right interval for each
element. The index indicators allow both the correct result position and
correct right interval index to be determined.

The time complexity of this algorithm in O(n * log(n)) where `n` is the length
of the input. This comes from both the initial sort and the `n` binary searches
to produce the result. The space complexity is O(n). This comes from making
the sorted list to facilitate the binary search.

```java
class Solution {
    private class Node {
        int[] interval;
        int index;
        Node(int[] interval, int index) {
            this.interval = interval;
            this.index = index;
        }
    }

    public int[] findRightInterval(int[][] intervals) {
        int n = intervals.length;
        Node[] nodes = new Node[n];
        for (int i = 0; i < n; i++) {
            nodes[i] = new Node(intervals[i], i);
        }

        Arrays.sort(nodes, (n1, n2) -> {
           return n1.interval[0] - n2.interval[0];
        });

        int[] result = new int[n];
        for (Node node : nodes) {
            result[node.index] = binsearch(nodes, node.interval[1]);
        }

        return result;
    }

    private int binsearch(Node[] nodes, int end) {
        int lo = 0;
        int hi = nodes.length - 1;
        int ret = -1;
        while (lo <= hi) {
            int mid = lo + (hi - lo) / 2;
            Node n = nodes[mid];
            if (n.interval[0] >= end) {
                ret = n.index;
                hi = mid - 1;
            } else {
                lo = mid + 1;
            }
        }
        return ret;
    }
}
```

### Sort two lists, single pass for joy

What if instead of using one sorted list we use two? For this solution we
produce a list sorted by start values and a list sorted by end values. In this
case we can take a pass through the list sorted by end values and we know that
the results can only be increasing through the list sorted by start values.
This gets rid of the binary search and allows linear passes to complete the
result.

The time complexity of this solution remains O(n * log(n)). While we have
eliminated the binary search we now have two sorts. The space complexity also
remains O(n).

```java
class Solution {
    private class Node {
        int[] interval;
        int index;
        Node(int[] interval, int index) {
            this.interval = interval;
            this.index = index;
        }
    }

    public int[] findRightInterval(int[][] intervals) {
        int n = intervals.length;
        Node[] byStart = new Node[n];
        Node[] byEnd = new Node[n];
        for (int i = 0; i < n; i++) {
            byStart[i] = byEnd[i] = new Node(intervals[i], i);
        }

        Arrays.sort(byStart, (o1, o2) -> o1.interval[0] - o2.interval[0]);
        Arrays.sort(byEnd, (o1, o2) -> o1.interval[1] - o2.interval[1]);

        int lo = 0;
        int[] result = new int[n];
        Arrays.fill(result, -1);
        for (Node node : byEnd) {
            int end = node.interval[1];
            while (lo < n) {
                Node startNode = byStart[lo];
                if (startNode.interval[0] >= end) {
                    result[node.index] = startNode.index;
                    break;
                } else {
                    lo++;
                }
            }
        }
        return result;
    }
}
```
