# Guess Number Higher or Lower

We are playing the Guess Game. The game is as follows:

I pick a number from `1` to `n`. You have to guess which number I picked.

Every time you guess wrong, I will tell you whether the number I picked is
higher or lower than your guess.

You call a pre-defined API `int guess(int num)`, which returns 3 possible
results:

*   `-1`: The number I picked is lower than your guess (i.e. `pick < num`).
*   `1`: The number I picked is higher than your guess (i.e. `pick > num`).
*   `0`: The number I picked is equal to your guess (i.e. `pick == num`).

## Hints

1. This has an obvious correlation to a standard algorithm. Do that.

## Solutions

### This is binary search

The problem as specified pretty much translates to a binary search.

The time complexity of this solution depends of course on the implementation
of `int guess(int num)` which is out of our control. If we expect no funny
business, as per a standard binary search this is O(log(n)). The space
complexity of this solution is likewise O(1).

```java
public class Solution extends GuessGame {
    public int guessNumber(int n) {
        int lo = 1;
        int hi = n;
        int pick = lo + (hi - lo) / 2;
        int ans = guess(pick);
        while (ans != 0) {
            if (ans > 0) {
                lo = pick + 1;
            } else {
                hi = pick - 1;
            }
            pick = lo + (hi - lo) /2;
            ans = guess(pick);
        }
        return pick;
    }
}
```
