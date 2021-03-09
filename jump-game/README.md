# Jump Game

Given an array of non-negative `num`, you are initially positioned at the
**first index** of the array.

Each element in the array represents your maximum jump length at that position.

Determine if you are able to reach the last index.

## Hints

1. This problem immediately brings to mind dynamic programming problems like
   minimum coins change problem, however, in this case you can't
   "look backwards" at specific coin sized gaps. The coin change problem is
   not helpful.
1. How about jumping forward with backtracking?

## Solutions

### Backtracking

Here we do one obvious backtracking approach through a recursive solution.
This solution will fail leeter's timers as it retries failed paths. We will
fix this in the next solution.

The approach basically checks the farthest it can hop via a recursive call and
if that route doesn't work backs off to farthest minus one.

```java
class Solution {
    public boolean canJump(int[] nums) {
        return canJump(nums, 0);
    }

    private boolean canJump(int[] nums, int from) {
        if (from == nums.length - 1) {
            return true;
        }
        int maxHop = Math.min(from + nums[from], nums.length - 1);
        for (int i = maxHop; i >= from + 1; i--) {
            if (canJump(nums, i)) {
                return true;
            }
        }
        return false;
    }
}
```

### Backtracking with a memo

Here we take the prior solution and speed it up by adding a memo. This makes it
a typical dynamic programming problem where the speed up comes from memoizing
failed paths so that they are not retried.

Note the usage of a `Boolean[]` for a tri-state, where `null` means unknown.

The space complexity for this solution is O(n). This comes both from the `memo`
used to record results and the recursive calls. The time complexity is O(n^2).

```java
class Solution {
    public boolean canJump(int[] nums) {
        Boolean[] memo = new Boolean[nums.length];
        memo[nums.length - 1] = true;
        return canJump(nums, 0, memo);
    }

    private boolean canJump(int[] nums, int from, Boolean[] memo) {
        Boolean can = memo[from];
        if (can != null) {
            return can;
        }
        int maxHop = Math.min(from + nums[from], nums.length - 1);
        for (int i = maxHop; i >= from + 1; i--) {
            if (canJump(nums, i, memo)) {
                memo[from] = Boolean.TRUE;
                return true;
            }
        }
        memo[from] = Boolean.FALSE;
        return false;
    }
}
```

### Last good index only

This solution flips things around and works from the backend of the array.
The last good index is tracked. This is the last index where we know we
can jump to the end. As we progress backwards we see if the current jump
value is greater than or equal to this last good index. If it is, then we
know we have found another good index. When we get to position `0`, it will
be known if we can jump to the end from the first position.

This solution is as good as we can do. The time complexity is O(n) as we
make a pass through the array. The space complexity is O(1) as we only have
one variable introduced, regardless of input size, used to track the last
good index.

```java
public class Solution {
    public boolean canJump(int[] nums) {
        // The last position is good as it is the finish.
        int last = nums.length - 1;
        for (int i = last - 1; i >= 0; i--) {
            if (i + nums[i] >= last) {
                last = i;
            }
        }
        return last == 0;
    }
}
```
