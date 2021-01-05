# X of a Kind in a Deck of Cards

In a deck of cards, each card has an integer written on it.

Return `true` if and only if you can choose `X >= 2` such that it is possible
to split the entire deck into 1 or more groups of cards, where:

* Each group has exactly `X` cards.
* All the cards in each group have the same integer.

## Hints

1. This problem statement is slightly tricky. Note that it didn't say that for
   each integer `y` all of its cards must be in a single group. For example if
   the deck was `[1, 1, 1, 1, 2, 2]` then a valid answer would be
   `[[1, 1], [1, 1], [2, 2]]`. That is two groups of two ones and one group of
   two twos.

## Solutions

### Group with a map

The solution here first counts each card with a `Map`. Note that under normal
circumstances you would use a `Multiset` but in standard Java there is no such
beast. With these counts in hand we figure out the card with the minimum count.
We do this because there would be no reason to check for groupings that are
outside of the range of `[2, min]`. Finally we do the check and report.

```java
class Solution {
    public boolean hasGroupsSizeX(int[] deck) {
        if (deck.length == 0) return false;

        // Determine the count of each card.
        Map<Integer, Integer> counts = new HashMap<>();
        for (int d : deck) {
            counts.put(d, counts.getOrDefault(d, 0) + 1);
        }

        // Determine the card with the minimum count.
        int min = Integer.MAX_VALUE;
        for (int i : counts.values()) {
            min = Math.min(min, i);
        }

        // Loop from our smallest to largest possible grouping,
        // stopping when we find one that works.
        for (int i = 2; i <= min; i++) {
            boolean good = true;
            for (int j : counts.values()) {
                if (j % i != 0) {
                    good = false;
                    break;
                }
            }
            if (good) return true;
        }

        return false;
    }
}
```

### Group with an array

For shits and giggles I wanted to see if grouping with an array would run faster
on leeter than the above solution. Since the constraints say that card values
are bound by `0 <= deck[i] < 10^4` it only requires an array of size 10000 to do
this.

It did in fact run through the tests faster than the above solution.

```java
class Solution {
    public boolean hasGroupsSizeX(int[] deck) {
        int[] counts = new int[10000];
        for (int d : deck) {
            counts[d]++;
        }

        int min = Integer.MAX_VALUE;
        for (int c : counts) {
            if (c > 0 && c < min) {
                min = c;
            }
        }

        for (int i = 2; i <= min; i++) {
            boolean good = true;
            for (int c : counts) {
                if (c % i != 0) {
                    good = false;
                    break;
                }
            }
            if (good) return true;
        }

        return false;
    }
}
```
