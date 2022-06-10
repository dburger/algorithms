# Sort an Array

Given an array of integers `nums`, sort the array in ascending order.

## Hints

1. Well it is a sort, remember your sorts?
1. While it is a sort, read the constraints!

## Solutions

### Constraints yield buckets

Looking at the constraints we can see that the possible values are limited
to [-5 * 10^4, 5 * 10^4]. Since this is an integer array size we can easily
allocate we can do this problem with a bucket sort. Here we initialize an
array, count into the buckets, and then fill back into the input array.

Using bucket sort the time and space complexity are O(1). The space used is
constant regardless of the input.

```java
class Solution {
    public int[] sortArray(int[] nums) {
        // Used to translate [-offset, offset] into [0, 2 * offset + 1].
        int offset = 5 * 10000;
        int[] buckets = new int[2 * offset + 1];
        for (int n : nums) {
            buckets[n + offset]++;
        }

        int fill = 0;
        for (int i = 0; i < buckets.length; i++) {
            int count = buckets[i];
            while (count-- > 0) {
                nums[fill++] = i - offset;
            }
        }
        return nums;
    }
}
```
