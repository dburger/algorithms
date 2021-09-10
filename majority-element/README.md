# Majority Element

Given an array `nums` of size `n`, return the majority element.

The majority element is the element that appears more than `Math.floor(n / 2)`
times. You may assume that the majority element always exists in the array.

## Hints

1. The brute force of checking each position is easy. It is O(n^2).
1. The brute force of using a counting map is easy. It is O(n).
1. The brute force of sorting and then counting the longest run is easy. It is
   O(n * log(n)).
1. Faster approaches take some insight.

## Solutions

### Boyer-Moore cast your votes

This leeter problem should not be listed as "easy". Reason being, you really
don't have a good solution until you get this one and this one takes a small
mental leap. This is called the "Boyer-Moore voting algorithm". It proceeds by
calling the numbers at position `0` the majority and maintaining a count for
it. Then it iterates through the rest of `nums`. If it sees the same majority
it merely increments. If it sees a different number it decrements. If it
decrements to `0` it accepts the number as the new majority. At the end of the
loop the majority element is guaranteed to remain. To see why note that
whenever we zero out and essentially throw away the front end of the array
we are at worst throwing out an equal number of majority and minority elements.
Therefore the majority is still the majority in the suffix that remains.

The time complexity for this solution is O(n) where n is the length of the
input array. The space complexity is O(1).

```java
class Solution {
    public int majorityElement(int[] nums) {
        int majority = nums[0];
        int count = 1;
        for (int i = 1; i < nums.length; i++) {
            int elem = nums[i];
            if (elem == majority) {
                count++;
            } else {
                if (--count == 0) {
                    majority = elem;
                    count = 1;
                }
            }
        }
        return majority;
    }
}
```
