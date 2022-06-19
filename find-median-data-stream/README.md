# Find Median from Data Stream

The **median** is the middle value in an ordered integer list. If the size of
the list is even, there is no middle value and the median is the mean of the
two middle values.

* For example for `arr = [2, 3, 4]`, the median is `3`.
* For example for `arr = [2,3]`, the median is `(2 + 3) / 2 = 2.5`.

Implement the MedianFinder class:

* `MedianFinder()` initializes the `MedianFinder` object.
* `void addNum(int num)` add the integer `num` from the data stream to the
  data structure.
* `double findMedian()` returns the median of all elements so far. Answers
  within `10^-5` of the actual answer will be accepted.

## Hints

1. Brute force, say sorting when needed, is easy but brutally slow.
   Time limit exceeded will result.
1. Could you keep two separate data structures of lows and highs?

## Solutions

### Get heapy with it

The problem can be solved by seperating the incoming stream into two groups.
One with the low half of numbers and the other with the high half. If, in
these structures we can retrieve the highest of the low and the lowest of the
high, we can produce the median. If the two groups are the same size the
median is the average of these two numbers. If the groups are off size by 1
the median is the value from the larger group. Thus the solution can be
acheived by maintaining these two groups in heaps as the numbers stream in.

The time complexity of this solution is dependent on the time complexity of
your heap implementation. Here we use Java's `PriorityQueue`. This makes
each `addNum(int num)` an O(log(n)) operation and `findMedian()` an O(1)
operation where `n` is the number of elements in the stream so far. The space
complexity is O(n) to hold the numbers in the two heaps.

```java
class MedianFinder {
    // For holding the low numbers, we need the max.
    private final PriorityQueue<Integer> maxHeap = new PriorityQueue<>((a, b) -> b - a);
    // For holding the high numbers, we need the min.
    private final PriorityQueue<Integer> minHeap = new PriorityQueue<>((a, b) -> a - b);

    public MedianFinder() {
    }

    public void addNum(int num) {
        // Add it to one of the heaps.
        if (maxHeap.isEmpty() || num < maxHeap.peek()) {
            maxHeap.offer(num);
        } else {
            minHeap.offer(num);
        }

        // Balance if necessary.
        int maxSize = maxHeap.size();
        int minSize = minHeap.size();
        if (maxSize > minSize + 1) {
            minHeap.offer(maxHeap.poll());
        }  else if (minSize > maxSize) {
            maxHeap.offer(minHeap.poll());
        }
    }

    public double findMedian() {
        if (maxHeap.size() == minHeap.size()) {
            return (maxHeap.peek() + minHeap.peek()) / 2.0;
        } else {
            return maxHeap.peek();
        }
    }
}
```
