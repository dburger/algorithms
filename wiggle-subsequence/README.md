# Wiggle Subsequence

A sequence of numbers is called a **wiggle sequence** if the differences between
successive numbers strictly alternate between positive and negative. The first
difference (if one exists) may be either positive or negative. A sequence with
fewer than two elements is trivially a wiggle sequence.

For example `[1, 7, 4, 9, 2, 5]` is a wiggle sequence because the differences
`(6, -3, 5, -7, 3)` are alternately positive and negative. In contrast,
`[1, 4, 7, 2, 5]` and `[1, 7, 4, 5, 5]` are not wiggle sequences, the first
because its first two differences are positive and the second because its last
difference is zero.

Given a sequence of integers, return the length of the longest subsequence that
is a wiggle sequence. A subsequence is obtained by deleting some number of
elements from the original sequence, leaving the elements in their original
order.

## Hints

1. A dynamic approach works here where we think in terms of the current diff at
   position i and whether it is a positive or negative diff. If it is positive,
   then the new max for wiggle sequences that end at i on a positive note is
   `down[i - 1] + 1`. If it is negative then the new max for wiggle sequences
   that end at i on a negative note is `up[i - 1] + 1`. The solution will be
   `max(down[n - 1], up[n - 1])` where `n` is the length of `nums`.
1. Don't let the "delete" in the description fool you. It is probably better to
   just think in terms of advancing through the sequence and tracking how many
   times you flip differences.

## Solutions

### Dynamic programmingn with the arrays stripped out

This solution follows the first hint. Since the suggested algorithm only needs
to look back at the prior up and down positions, the arrays are unnecessary and
only a last value need be kept.

The time complexity for this solution is O(n), where `n` is the length of the
`nums` array. The space complexity is O(1).

```java
class Solution {
    public int wiggleMaxLength(int[] nums) {
        if (nums.length < 2) {
            return nums.length;
        }
        int down = 1;
        int up = 1;
        for (int i = 1; i < nums.length; i++) {
            int diff = nums[i] - nums[i - 1];
            if (diff > 0) {
                up = down + 1;
            } else if (diff < 0) {
                down = up + 1;
            }
        }
        return Math.max(down, up);
    }
}
```

### Greedy tracking

The problem turns out to be equivalent to finding the number of alternating max
and min peaks in the array. This can be done be tracking a `priorDiff` value
and incrementing the count when the new `diff` flips the sign.

The time complexity of this solution is O(n), where `n` is the length of the
`nums` array. The space complexity is O(1).

```java
class Solution {
    public int wiggleMaxLength(int[] nums) {
        if (nums.length < 2) {
            return nums.length;
        }

        int priorDiff = nums[1] - nums[0];
        int count = priorDiff == 0 ? 1 : 2;
        for (int i = 2; i < nums.length; i++) {
            int diff = nums[i] - nums[i - 1];
            // Note the equal to zero part is for the first iteration, where
            // priorDiff may have been zero.
            if (diff != 0 && diff * priorDiff <= 0) {
                count++;
                // Of course only advance the prior on a new diff.
                priorDiff = diff;
            }
        }
        return count;
    }
}
```
