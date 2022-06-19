# Rotate Array

Given an array, rotate the array to the right by `k` steps, where `k` is
non-negative.

## Hints

1. The obvious is not hard but not in place.
1. Clever as hell - I don't even know how to hint.

## Solutions

### The obvious, not in place

Using modular arithmethic we can determine new positions from existing
positions. This allows us to program a very easy solution that is
O(n) time complexity and O(n) space complexity by working in a temporary
copy.

```java
class Solution {
    public void rotate(int[] nums, int k) {
        int[] res = new int[nums.length];
        for (int i = 0; i < nums.length; i++) {
            res[(i + k) % nums.length] = nums[i];
        }
        for (int i = 0; i < nums.length; i++) {
            nums[i] = res[i];
        }
    }
}
```

### Put 'er in reverse

If you see this insight you can get the clever as hell in place solution. First,
think of normalizing the rotation. That is if the rotation is greater than the
length of the list the actual result is a rotation of `k = k % nums.length`. Now
the end state will be the last `k` numbers being at the start of the list, and
the first `n - k` numbers being at the end of the list. This can be achieved
with 3 reversals. First reverse the list. Then reverse the first `k` elements.
Then reverse the last `n - k` elements.

The time complexity of this solution is O(n) where `n` is the length of the
input. The space complexity of this solution is O(1).

```java
class Solution {
    private void reverse(int[] nums, int lo, int hi) {
        while (lo < hi) {
            int temp = nums[hi];
            nums[hi] = nums[lo];
            nums[lo] = temp;
            lo++;
            hi--;
        }
    }

    public void rotate(int[] nums, int k) {
        reverse(nums, 0, nums.length - 1);
        int split = k % nums.length - 1;
        reverse(nums, 0, split);
        reverse(nums, split + 1, nums.length - 1);
    }
}
```
