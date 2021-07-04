# Best Time to Buy and Sell Stock

You are given an array `prices` where `prices[i]` is the price of a given
stock on the `i`th day. You want to maximize your profit by choosing a
**single day** to buy one stock and choosing a **different day in the future**
to sell that stock.

Return the *maximum profit you can achieve* from this transaction. If you
cannot achieve any profit, return `0`.

## Hints

1. The maximum profit calculation for each given day is obviously the day's
   price minus the minimum seen so far. This suggest an algorithm.

## Solutions

### Low so far, how high can we go?

This solution follows the hint. `lo` is set to the value in the first element.
`max` is set to `Integer.MIN_VALUE`. Then we iterate through the remaining
elements in the array. In each iteration we calculate the maximum profit that
could be had on that day if we bought on the lowest `lo` before it. If that
beats what we have seen so far we record that. Then we make sure to update
the `lo` if necessary.

This solution has O(n) time complexity from one pass through the array. The
space complexity is O(1) as two temp variables are kept regardless of the size
of the input.

```java
class Solution {
    public int maxProfit(int[] prices) {
        int lo = prices[0];
        int maxProfit = Integer.MIN_VALUE;
        for (int i = 1; i < prices.length; i++) {
            int price = prices[i];
            int profit = price - lo;
            if (profit > maxProfit) {
                maxProfit = profit;
            }
            if (price < lo) {
                lo = price;
            }
        }
        return maxProfit < 0 ? 0 : maxProfit;
    }
}
```
