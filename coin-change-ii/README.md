# Coin Change II

You are given an integer array `coins` representing coins of different and an
integer `amount` representing a total amount of money.

Return the *number of combinations that make up that amount*. If that amount of
money cannot be made up by any combination of the coins, return `0`.

You may assume that you have an infinite number of each kind of coin.

The answer is **guaranteed** to fit into a signed **32-bit** integer.

## Hints

1. Beware the combinations, (not permutations!).

## Solutions

### Naive DP, does not work

**Note that this does not work.** If you have done any amount of dynamic
programming your first inclination might be to code this up like this:

```java
public int change(int amount, int[] coins) {
    int[] dp = new int[amount + 1];
    dp[0] = 1;
    for (int i = 1; i < dp.length; i++) {
        for (int c : coins) {
            int offset = i - c;
            if (offset >= 0) {
                dp[i] += dp[offset];
            }
        }
    }
    return dp[amount];
}
```

This seems like an ok approach but this **does not work**. Do you see why?

The reason why is that the problem asks you to count the combinations of the
given `coins` that add up to `amount`. What the above does it to count the
permutations of `coins` that add up to `amount`. To make this very clear the
above algorithm will count `{1, 2}` and `{2, 1}` as two separate solutions
for the value of `3`.

### Track the actual coins

Given the problem of the prior solution, instead of tracking the counts,
tracking the actual `Set`s of `List`s of coins should work to eliminate the
duplicates.

Unfortunately the time complexity of this solution is not great. Each coin
must be considered at each position in the array. When we add a combination
it must be sorted in order for the `Set` approach to reject duplicates. This
all adds to an approach that **won't pass the leeter time constraints**.

```java
class Solution {
    public int change(int amount, int[] coins) {
        List<Set<List<Integer>>> l = new ArrayList<>();
        Set<List<Integer>> zero = new HashSet<>();
        zero.add(Collections.emptyList());
        l.add(zero);
        for (int i = 1; i < amount + 1; i++) {
            Set<List<Integer>> temp = new HashSet<>();
            for (int c : coins) {
                int pos = i - c;
                if (pos >= 0) {
                    for (List<Integer> existing : l.get(pos)) {
                        List<Integer> copy = new ArrayList<>(existing);
                        copy.add(c);
                        Collections.sort(copy);;
                        temp.add(copy);
                    }
                }
            }
            l.add(temp);
        }
        return l.get(amount).size();
    }
}
```

### Better dynamic programming

By inverting the amount / coins nesting of our first attempt we get a dynamic
programming approach that avoids duplicate combinations.

The time complexity of this solution is O(n * amount) where n is the number of
coins. The space complexity of this solution is O(amount).

```java
class Solution {
    public int change(int amount, int[] coins) {
        int[] dp = new int[amount + 1];
        dp[0] = 1;
        for (int c : coins) {
            for (int i = c; i < dp.length; i++) {
                dp[i] += dp[i - c];
            }
        }
        return dp[amount];
    }
}
```
