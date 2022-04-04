# Beautiful Arrangement

Suppose you have `n` integers labeled `1` through `n`. A permutation of those
`n` integers `perm` (1-indexed) is considered a **beautiful arrangement** if
for every `i (1 <= i <= n)`, **either** of the following is true:

- `perm[i]` is divisible by `i`
- `i` is divisible by `perm[i]`

Given an integer `n`, return the number of the beautiful arrangements that you
can construct.

## Hints

1. This falls under the class of algorithms that can be solved via
   backtracking.

## Solutions

### Track that back

Backtracking works nicely here. Proceed by incrementing through the valid
numbers and placing a number when it meets the modulus contraints. The
backtracking occurs when we remove the placed number and try the next number.

The space complexity for this solution is simple. The `occupied` array keeps
track of placements, and must be sized to O(n). The time complexity is a
different story. The time complexity is a bit trickier. I'll revisit this in
a later fix up.

TODO(dburger): Time complexity.

```java
class Solution {
    public int countArrangement(int n) {
        int[] result = new int[1];
        boolean[] occupied = new boolean[n + 1];
        count(1, occupied, result);
        return result[0];
    }

    private void count(int pos, boolean[] occupied, int[] result) {
        if (pos == occupied.length) {
            result[0]++;
            return;
        }

        for (int i = 1; i < occupied.length; i++) {
            if (occupied[i]) {
                continue;
            }
            if (i % pos == 0 || pos % i == 0) {
                occupied[i] = true;
                count(pos + 1, occupied, result);
                occupied[i] = false;
            }
        }
    }
}
```
