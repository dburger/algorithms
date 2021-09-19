# Two Sum II

Return the indices of the two numbers (**1-index**) as an integer array
`answer` of size `2`, where `1 <= answer[0] < answer[1] <= numbers.length`.

The tests are generated such that there is **exactly one solution**. You
**may not** use the same element twice.

## Hints

1. This can be handled with a closing window approach.

## Solutions

### Close that window

This solution is the obvious closing window approach. We start with pointers
to the front and end of `numbers` and then adjust the end per the current sum.

The time complexity of this solution is O(n) where n is the size of `numbers`.
This comes from the possibility of travsering the entire list to close in on
the solution. The space complexity is O(1).

```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int l = 0;
        int r = numbers.length - 1;
        for (;;) {
            int v1 = numbers[l];
            int v2 = numbers[r];
            int sum = v1 + v2;
            if (sum > target) {
                r--;
            } else if (sum < target) {
                l++;
            } else {
                return new int[] {l + 1, r + 1};
            }
        }
    }
}
```
