# Interleave Array

Given the array `nums` consisting of `2n` elements in the form
`[x1,x2,...,xn,y1,y2,...,yn]`, return the array in the form
`[x1,y1,x2,y2,...,xn,yn]`.

## Hints

1. Pretty straightforward. Maintain your indices.

## Solutions

### Track em all

This solution merely tracks all the points of interest, the destination,
the `x` item, and the `y` item.

The big O running time and space complexity for this algorithm is O(n). The
time complexity comes from iterating through the array to make the copy. The
space complexity comes from construction of the copy.

```java
class Solution {
    public int[] shuffle(int[] nums, int n) {
        int[] ret = new int[2 * n];
        int front = 0;
        int tail = n;
        int dest = 0;
        while (dest < 2 * n) {
            ret[dest++] = nums[front++];
            ret[dest++] = nums[tail++];
        }
        return ret;
    }
}
```
