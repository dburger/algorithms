# Coin Change

You are given coins of different denominations and a total amount of
money `amount`. Write a function to computer the fewest number of coins
that you need to make up that amount. If that amount of money cannot
be made up by any combination of the coins, return `-1`.

You may assume that you have an infinite number of each kind of coin.

## Hints

1. This is the classic dynamic programming problem. Think of how you could
   for amount `n`, if you knew the solution for amounts `[0, n - 1]`. Then
   realize this approach in an array.

## Solutions

### Dynamic programming

This classic dynamic programming approach is O(len(coins) * amount).

```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        // By sorting the coins we can avoid checking larger coins if they
        // result in a negative index.
        Arrays.sort(coins);
        int[] memo = new int[amount + 1];
        Arrays.fill(memo, Integer.MAX_VALUE);
        memo[0] = 0;
        for (int i = 1; i < memo.length; i++) {
            for (int coin : coins) {
                int idx = i - coin;
                if (idx < 0) {
                    // Index to check is negative. This coin and larger coins
                    // are invalid at this amount.
                    break;
                }
                int best = memo[idx];
                if (best < Integer.MAX_VALUE) {
                    // Amount at idx has a solution, see if adding this coin,
                    // +1 on the old best, is better than what is already here.
                    memo[i] = Math.min(memo[i], best + 1);
                }
            }
        }
        // The result for amount is at the last position, if possible.
        int result = memo[memo.length - 1];
        return result == Integer.MAX_VALUE ? -1 : result;
    }
}
```
