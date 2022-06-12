# Element Appearing More Than 25% in Sorted Array

Given an integer array **sorted** in non-decreasing order, there is exactly one
integer in the array that occurs more than 25% of the time, return that
integer.

## Hints

1. It's sorted. You can count.

## Solutions

### Count and run

This is rated as an easy problem and with it already sorted that makes an
O(n) solution easy to achieve. Here we merely keep a count and if it goes above
the 25% threshold we are done.

The time complexity for this algorithm is O(n) where `n` is the size of the
input array as you may need to traverse the entire array. The space complexity
of this algorithm is O(1). The same space is required regardless of the input.

```java
class Solution {
    public int findSpecialInteger(int[] arr) {
        // By the problem definition, if the length is less than
        // 4 the first element has > 25% of the count.
        if (arr.length < 4) {
            return arr[0];
        }

        int threshold = arr.length / 4;
        int prev = arr[0];
        int count = 1;
        for (int i = 1; i < arr.length; i++) {
            int curr = arr[i];
            if (curr == prev) {
                count++;
            } else {
                prev = curr;
                count = 1;
            }
            if (count > threshold) {
                return curr;
            }
        }

        // Problem states there is always an answer, it won't get here.
        return -1;
    }
}
```
