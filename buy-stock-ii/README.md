# Best Time to Buy and Sell Stock II

You are given an array `prices` where `prices[i]` is the price of a given
stock on the `i`th day.

Find the maximum profit you can achieve. You may complete as many transactions
as you like (i.e., buy one and sell one share of the stock multiple times).

**Note:** You may not engage in multiple transactions simultaneously (i.e.,
you must sell the stock before you buy again).

## Hints

1. Count the rising?

## Solutions

### Climb those peaks, and count em

This solution merely recognizes that to maximize stock proft in this scenario
you would want to be holding on any day that the stock is going up. Likewise
you would sell on any day where the stock was to go down the next day. With
this perfect vision we can just "sum the upticks."

The time complexity of this solution is O(n) to make a run through the list.
The space complexity of this solution is O(1). Only a temp variable is used
to hold the `total`.

```java
class Solution {
    public int maxProfit(int[] prices) {
        int total = 0;
        for (int i = 0; i < prices.length - 1; i++) {
            int diff = prices[i + 1] - prices[i];
            if (diff > 0) {
                total += diff;
            }
        }
        return total;
    }
}
```
