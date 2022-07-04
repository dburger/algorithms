# Count Bits

Given an integer `n`, return an array `ans` of length `n + 1` such that for
each `i (0 <= i <=n)`, `ans[i]` is the **number of** `1`'s in the binary
representation of `i`.

Can you do it in O(n)?

## Hints

1. You could just count the bits.
1. Can you build from previous results?

## Solutions

### Lazy and easy

Well the intention here is to be O(n) I suppose this counts as the number of
bits in an `int` is limited to 32 and the`bitCount` method at worst loops
through them all.

So I'm saying the time complexity of this is O(n) and the space complexity is
O(1).

```java
class Solution {
    public int[] countBits(int n) {
        int[] result = new int[n + 1];
        for (int i = 0; i <= n; i++) {
            result[i] = Integer.bitCount(i);
        }
        return result;
    }
}
```

### Jeez, at least count your bits

The last solution wasn't very satisfying. Here we at least take the effort
to write our own bit counting code.

The time and space complexity remain O(n) and O(1) respecitvely.

```java
class Solution {
    public int[] countBits(int n) {
        int[] result = new int[n + 1];
        for (int i = 0; i <= n; i++) {
            result[i] = bitCount(i);
        }
        return result;
    }

    private int bitCount(int val) {
        int count = 0;
        while (val != 0) {
            val = val & (val - 1);
            count++;
        }
        return count;
    }
}
```

### Use prior solutions, kind of DP

Many efficient algorithms are effificient because they can reuse prior
solutions and thus reduce computation. How could we do this here? Well, one
way is that we know that each power of 2 has only one bit. For things that are
not powers of two we can actually split them into two pieces. The high bit by
itself which is a power of two, and the remainder, which is some smaller value.
Thus we have the split we need. For non-powers of 2 `val` the bit count is
`1 + bitCount(val % last power of 2)`.

As sexy as this is the time complexity remains O(n). We kind of did some hand
waving above and said that because `bitCount` will do a max of 32 steps on
any input we are going to call that O(1) for that step. In practice not
recounting bits here should give a small boost. The space complexity remains
O(1).

```java
class Solution {
    public int[] countBits(int n) {
        int[] results = new int[n + 1];
        if (n < 1) {
            return results;
        }
        results[1] = 1;
        int lastPow2 = 1;
        int nextPow2 = 2;
        for (int i = 2; i <= n; i++) {
            if (i == nextPow2) {
                results[i] = 1;
                lastPow2 = nextPow2;
                nextPow2 *= 2;
            } else {
                results[i] = 1 + results[i % lastPow2];
            }
        }
        return results;
    }
}
```
