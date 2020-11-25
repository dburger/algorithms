# Water Bottles

Given `numBottles` full water bottles, you can exchange `numExchange` empty
water bottles for one full water bottle.

The operation of drinking a full water bottle turns it into an empty bottle.

Return the **maximum** number of water bottles you drink.

## Hints

1. Division and modulus should get you through. This is much like a math problem
   where you need to keep track of the "carry" (as yet unused empties).

## Solutions

### Drink and drink

```java
class Solution {
    public int numWaterBottles(int numBottles, int numExchange) {
        int numDrank = 0;
        int numEmpties = 0;
        while (numBottles > 0) {
            // Drink and adjust.
            numDrank += numBottles;
            numEmpties += numBottles;
            // Redeem.
            numBottles = numEmpties / numExchange;
            numEmpties = numEmpties % numExchange;
        }
        return numDrank;
    }
}
```
