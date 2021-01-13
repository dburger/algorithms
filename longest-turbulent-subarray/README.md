# Longest Turbulent Subarray

Given an integer array `arr`, return the length of the maximum size turbulent
subarray of `arr`.

A subarray is **turbulent** if the comparison sign flips between each adjacent
pair of elements in the subarray.

## Hints

1. Dynamic programming with an array that holds the longest turbulent subarray
   up to index i should do the trick.
1. Iterate from left to right and keep track of where turbulence starts. It ends
   when the sign does not flip or you reach the end of the array.

## Solutions

### Dynamic programming

Here we have the dynamic programming approach. It seeds the `dp` array with a
one at index 0. That is, a single array item has a turbulent subarray of length
1. Then at each position starting at 1, it looks to see if the current item is
equal, less, or greater than the prior. If it is equal, it resets `dp[i] = 1`.
If they are not equal it needs to check the prior value. If that indicates a
continuation of the prior sequence then the result is `dp[i] = dp[i - 1] + 1`.
If it is not a continuation then `dp[i] = 2` as the last 2 numbers are starting
a new sequence.

The time complexity of this solution is O(n) where n is the length of the array.
The space complexity is O(n), as it allocates the dynamic programming array.

```java
class Solution {
    public int maxTurbulenceSize(int[] arr) {
        if (arr.length < 2) {
            return arr.length;
        }

        int[] dp = new int[arr.length];
        dp[0] = 1;
        int max = 1;

        for (int i = 1; i < arr.length; i++) {
            int val1 = arr[i - 1];
            int val2 = arr[i];

            if (val1 == val2) {
                dp[i] = 1;
            } else if (i == 1) {
                // No prior yet.
                dp[i] = 2;
            } else {
                int prior = arr[i - 2];
                if (val1 < val2) {
                    if (prior > val1) {
                        // Continuation of previous.
                        dp[i] = dp[i - 1] + 1;
                    } else {
                        // Starting a new one with last two.
                        dp[i] = 2;
                    }
                } else {
                    if (prior < val1) {
                        // Continuation of previous.
                        dp[i] = dp[i - 1] + 1;
                    } else {
                        // Starting a new one with last two.
                        dp[i] = 2;
                    }
                }
            }
            max = Math.max(max, dp[i]);
        }

        return max;
    }
}
```

### Dynamic programming with fancier comparison

The above solution is good but quite verbose in the `val1 != val2` part. Here
we get a bit fancier using Java's `Integer.compare` routine. It is ever so
slightly slower than the above because of the added function calls, but it
reduces some of the duplication. Here the determination if it is a continuation
of a prior sequence uses the multiplication of the two comparisons being `-1`.
That is, they are different, one `-1` indicating less than, and one `+1`
indicating greater than.

The time and space complexity are the same as the above.

```java
class Solution {
    public int maxTurbulenceSize(int[] arr) {
        if (arr.length < 2) {
            return arr.length;
        }

        int[] dp = new int[arr.length];
        dp[0] = 1;
        int max = 1;

        for (int i = 1; i < arr.length; i++) {
            int val1 = arr[i - 1];
            int val2 = arr[i];

            if (val1 == val2) {
                dp[i] = 1;
            } else if (i == 1) {
                // No prior yet.
                dp[i] = 2;
            } else {
                int prior = arr[i - 2];
                int cmp = Integer.compare(val1, val2);
                if (Integer.compare(prior, val1) * cmp == -1) {
                    dp[i] = dp[i - 1] + 1;
                } else {
                    dp[i] = 2;
                }
            }
            max = Math.max(max, dp[i]);
        }

        return max;
    }
}
```

### Mark start and run

This is rather clever. I snarfed this from another leeter answer. Here we keep
track of `start` (of a subarray) and `max`. In the loop we compare the current
to the prior. If they are the same, we merely adjust start. If they are not
the we check if we are hitting the end of the array or the next value will stop
the sequence. In these cases we calculate a `max` (since the run is stopped) and
set a new `start`.

The key little insight here is how this one looks **forward** to capture a `max`
and adjust `start`.

This is O(n) time complexity like the dynamic programming solution but features
and improve O(1) space complexity.

```java
class Solution {
    public int maxTurbulenceSize(int[] arr) {
        int n = arr.length;
        if (n < 2) {
            return n;
        }

        int start = 0;
        int max = 1;

        for (int i = 1; i < n; i++) {
            int cmp = Integer.compare(arr[i - 1], arr[i]);
            if (cmp == 0) {
                start = i;
            } else if (i == n - 1 ||
                    Integer.compare(arr[i], arr[i + 1]) * cmp != -1) {
                max = Math.max(max, i - start + 1);
                start = i;
            }
        }
        return max;
    }
}
```
