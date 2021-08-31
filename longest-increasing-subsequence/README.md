# Longest Increasing Subsequnce

A **subsequence** is a sequence that can be derived from an array by deleting
some or no elements without changing the order of the remaining elements. For
example, `[3, 6, 2, 7]` is a subsequence of the array `[0, 3, 1, 6, 2, 2, 7]`.

## Hints

1. Looking at a position `i`, the max run up to that number is the max run for
   any position `j` to the left of that number where `nums[j] < nums[i]`. This
   leads to a dynamic programming like solution.

## Solutions

### Looking back

This solution follows the hint above. We create an array and assign a max run
of `1` to the `0`th position. Then we iterate through remaining positions
setting each `i`'s max run to the max of `1` and `seq[j] + 1` for any position
`j` where `nums[i] > nums[j]`.

While having hints of a dynamic programming like solution the time complexity
of this solution is O(n^2). That is at each position `i` we may need to do `n`
checks back to the front of the array. The space complexity of this solution
is also O(n), as we have the `seq` array sized to hold a max sequence count
up to each position in the array.

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int max = 1;
        int[] seq = new int[nums.length];
        seq[0] = 1;
        for (int i = 1; i < nums.length; i++) {
            int iVal = nums[i];
            int m = 1;
            for (int j = i - 1; j > -1; j--) {
                if (iVal > nums[j]) {
                    int n = seq[j] + 1;
                    if (n > m) {
                        m = n;
                        if (m > max) {
                            max = m;
                            break;
                        }
                    }
                }
            }
            seq[i] = m;
        }
        return max;
    }
}
```
