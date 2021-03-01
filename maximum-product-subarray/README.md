# Maximum Product Subarray

Given an integer array `nums`, find the contiguous subarray within an
array (containing at least one number) which has the largest product.

## Hints

1. Keep track of current max versus global max. There is a complexity here
   that a current negative product can be flipped to a positive product with
   multiplication by a negative. How are you going to handle that?

## Solutions

### Max and min runs

This solution keeps track of the global maximum product and the current maximum
and minimum products. The minimum is kept because it can become the maximum with
multiplication by the next negative integer (As the maximum can flop to the
minimum as well).

The time complexity of this algorithm is O(n) as it must iterate through all
values. The space complexity is O(1) as only 3 tracking ints are required
regardless of the input size.

```java
class Solution {
    public int maxProduct(int[] nums) {
        int currMax = nums[0];
        int currMin = nums[0];
        int result = nums[0];
        for (int i = 1; i < nums.length; i++) {
            int val = nums[i];
            int tmp1 = val * currMax;
            int tmp2 = val * currMin;
            currMax = Math.max(val, Math.max(tmp1, tmp2));
            currMin = Math.min(val, Math.min(tmp1, tmp2));
            result = Math.max(result, currMax);
        }
        return result;
    }
}
```
