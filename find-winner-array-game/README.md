# Find the Winner of an Array Game

Given an integer array `arr` of **distinct** integers and an integer k, a game
will be played between the first two elements of the array (`arr[0]` and
`arr[1]`). In each round of the game, we compare these two elements, the larger
integer wins and is placed at position `0`, the smaller integer moves to the end
of the array. The game ends when an integer wins `k` consecutive rounds.

Return the integer which will win the game.

It is **guaranteed** that there will be a winner of the game.

## Hints

1. If you brute forced this and followed the process as described the shifting
   of elements would become the expensive part of the routine. Thus pointing
   at the candidate and challenger would be preferred.
1. If `k` is big enough then the largest element in the array will be
   encountered and win from there on out. This is an obvious optimization that
   should be done.
1. Whenever you hit the end of the array you are done. At that point you've seen
   every candidate and your current winner will continue to win.

## Solutions

### Candidate and challenger

This solution follows the hints above. First, if `k` is greater than or equal to
`n - 1`, where `n` is the length of `arr`, then every element is going to get
a chance to compete. In that case the maximum value will always end up winning.
Thus a short circuit that just takes the maximum from the array. If that doesn't
hold, we merely track a candidate and challenger, incrementing the `wins` or
setting it back to one depending on who wins. We continue until we hit `k`
wins or the end of the array. If we hit `k` wins the game is obviously over. If
we hit the end of the array we have encountered every element and the highest of
those elements has been identified and will continue to win. In either case, the
game is obviously determined.

The time complexity of this solution is O(n) where `n` is the length of `arr` as
we may need to examine every element of the array. The space complexity is O(1).
Note that the short circuit is not necessary to get to O(n), but it does provide
a nice little speed up in practice.

```java
class Solution {
    public int getWinner(int[] arr, int k) {
        int n = arr.length;
        if (k >= n - 1) {
            return max(arr);
        }
        int candidate = arr[0];
        int i = 1;
        int wins = 0;
        while (wins < k && i < n) {
            int challenger = arr[i++];
            if (candidate > challenger) {
                wins++;
            } else {
                candidate = challenger;
                wins = 1;
            }
        }
        return candidate;
    }

    private int max(int[] nums) {
        int max = nums[0];
        for (int i = 1; i < nums.length; i++) {
            int val = nums[i];
            if (val > max) {
                max = val;
            }
        }
        return max;
    }
}
```
