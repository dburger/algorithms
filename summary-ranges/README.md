# Summary Ranges

You are given a **sorted unique** integer array `nums`.

A **range** `[a, b]` is the set of all integers from `a` to `b` (inclusive).

Return the **smallest sorted** list of ranges that **cover all the numbers in
the array exactly**. That is, each element of `nums` is covered by exactly
one of the ranges, and there is no integer `x` such that `x` is in one of the
ranges but not in `nums`.

Each range `[a, b]` in the list should be output as:

- `"a->b"` if `a != b`
- "a" if `a == b'

## Hints

1. No hints necessary.

## Solutions

### Simply crank it

This solution proceeds in the obvious fashion. Since the input is already
sorted and unique, we merely keep track of `start` and `prev`, and advance
through the array. When we see a skip, we record the result. A little
twist here is that at the termination of the loop we must finish with
adding in the final range.

The time complexity of this solution is O(n) where n is the length of the
input. The space complexity is O(1)....err the space complexity requires
the result though and it will be O(n) as each element in the input could
require its own range.

```java
class Solution {
    public List<String> summaryRanges(int[] nums) {
        List<String> results = new ArrayList<>();
        if (nums.length == 0) {
            return results;
        }
        int start = nums[0];
        int prev = start;
        for (int i = 1; i < nums.length; i++) {
            int curr = nums[i];
            if (curr != prev + 1) {
                if (start == prev) {
                    results.add(String.valueOf(start));
                } else {
                    results.add(start + "->" + prev);
                }
                start = curr;
            }
            prev = curr;
        }
        if (start == prev) {
            results.add(String.valueOf(start));
        } else {
            results.add(start + "->" + prev);
        }
        return results;
    }
}
```
