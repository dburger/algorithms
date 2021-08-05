# Random Pick with Weight

You are given an array of positive integers `w` where `w[i]` describes the
weight of the `i`th index (0-indexed).

We need to call the function `pickIndex()` which **randomly** returns an
integer in the range `[0, w.length - 1]`. `pickIndex()` should return the
integer proportional to its weight in the `w` array. For example, for
`w = [1, 3]`, the probability of picking the index `0` is
`1 / (1 + 3) = 0.25` (i.e. 25%) while the probability of picking the `1`
index is `3 / (1 + 3) = 0.75` (i.e. 75%).

## Hints

1. A brute force approach would need to pick a random number `p` of the
   appropriate range and then walk through `w` until it finds the position
   that `p` falls in.
2. The natural extension to the prior hint is to precompute the position
   ranges and binary search to the correct position.

## Solutions

### Walk the walk

A brute force way to approach this problem is to pre-calulate the overall sum
and store references to that sum and the original input array `w`. Then, on
`pickIndex()` invocations, a random value `p` in the range `[1, sum]` is
generated and then we walk `w` at index `i` until
`p < the cumulative sum up to i`.

The time complexity of the construction of this class is O(n) where n is the
size of `w`. This is to compute the sum. The time complexity of each invocation
of `pickIndex()` is likewise O(n) to walk the array. The space complexity of
this solution is O(1) as there are only a constant number of variables needed
in regardless of the size of the inputs.

```java
class Solution {
    private final int sum;
    private final int[] w;
    private final Random r = new Random();

    public Solution(int[] w) {
        int sum = 0;
        for (int n : w) {
            sum += n;
        }
        this.sum = sum;
        this.w = w;
    }

    public int pickIndex() {
        // Generate random in range [1, sum]
        int val = r.nextInt(sum) + 1;
        int cumulative = 0;
        for (int i = 0; i < this.w.length; i++) {
            cumulative += this.w[i];
            if (val < cumulative) {
                return i;
            }
        }
        // Can't get here.
        return 0;
    }
}
```

### Binary search the walk

This is a good example where the brute force solution suggests a better
solution. Note that in the every `pickIndex()` method we end up computing
the cumulative sum for `w`. If we pre-compute these cumulative sums or the
prefix sum, then we can obviously binary search to the correct position.

This keeps the construction at O(n) as the same sum must be computed. The
time complexity of `pickIndex()` is reduced to O(log(n)). The space complexity
of this solution is O(n) to hold the `prefix` sum.

```java
class Solution {
    private final int sum;
    private final int[] prefix;
    private final Random r = new Random();

    public Solution(int[] w) {
        int sum = 0;
        int[] prefix = new int[w.length];
        for (int i = 0; i < w.length; i++) {
            sum += w[i];
            prefix[i] = sum;
        }
        this.sum = sum;
        this.prefix = prefix;
    }

    public int pickIndex() {
        // Generate random in range [1, sum]
        int pick = r.nextInt(sum) + 1;
        return bs(pick);
    }

    private int bs(int pick) {
        int lo = 0;
        int hi = prefix.length - 1;
        while (lo < hi) {
            int mid = lo + (hi - lo) / 2;
            int val = this.prefix[mid];
            if (pick > val) {
                lo = mid + 1;
            } else {
                hi = mid;
            }
        }
        return lo;
    }
}
```
