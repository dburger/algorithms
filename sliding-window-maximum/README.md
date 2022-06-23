# Sliding Window Maximum

You are given an array of integers `nums`, there is a sliding window of size `k`
which is moving from the very left of the array to the very right. You can only
see the `k` numbers in the window. Each time the sliding window moves right by
one position.

Return an array containing the max in each window.

## Hints

1. A correctly controlled max heap would work, right?

## Solutions

### Heap it up!

The hint above is just too direct. Yes, a heap approach will work nicely for
this problem. There is a catch, however. If you just place the integer in the
heap without regard to original positions in `nums`, then to remove the items
that are sliding out of the window you have to do a `remove(Object)` call.
These removals are O(n). So the performance will suffer.

Is there a way around this? Yes, actually a very easy way. Instead of adding
just the integer to the heap we can add an object that tracks both the value
and its original position in the input. Then when we are selecting the max for
a window we check if the index position of the proposed next element is in the
required range. If not, we just skip that element. These removals are
O(log(n)).

The time complexity of this solution is O(n * log(n)) where `n` is the length
of the input. This comes from needing to add each element to the heap and
potentially remove each element from the heap. The space complexity is O(n) to
hold the elements in the heap.

```java
class Solution {
    class Element {
        final int index;
        final int value;

        Element(int index, int value) {
            this.index = index;
            this.value = value;
        }
    }

    public int[] maxSlidingWindow(int[] nums, int k) {
        PriorityQueue<Element> pq = new PriorityQueue<>((a, b) -> {
            int res = b.value - a.value;
            return res == 0 ? a.index - b.index : res;
        });

        int fill = 0;
        int[] result = new int[nums.length - k + 1];
        for (int i = 0; i < nums.length; i++) {
            pq.offer(new Element(i, nums[i]));
            if (pq.size() >= k) {
                while (true) {
                    Element e = pq.peek();
                    if (e.index < i - k + 1) {
                        // Max in the heap, but no longer in window, throw away.
                        pq.poll();
                    } else {
                        // Max in the heap, and in the window, record it.
                        result[fill++] = e.value;
                        break;
                    }
                }
            }
        }

        return result;
    }
}
```
