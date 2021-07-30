# Contiguous Array

Given a binary array `nums`, return the maximum length of a contiguous subarray
with an equal number of `0` and `1`.

## Hints

1. An equal number of `0` and `1` is indicated by any stretch where the `+/-`
   count returns to the same value.

## Solutions

### Count that max

Here we follow the strategy indicated in the hint. We keep track of a running
`+/-` count. That is a value of `1` is translated to a `+1` and a `0` is
translated to a `-1`. We also keep a map that tracks
`count -> lowest index seen at`. At each count seen, if it has been seen before
we check if this new occurence beats `max`. If it hasn't been seen before it
is recorded as the new `lowest index seen at`.

The time complexity of this algorithm is O(n) coming from the pass through the
array. The space complexity is O(n) to keep track of the lowest index seen at
for counts.

```java
class Solution {
    public int findMaxLength(int[] nums) {
        // count -> lowest index seen at
        Map<Integer, Integer> counts = new HashMap<>();
        // Make the implicit 0 count explicit.
        counts.put(0, -1);
        int count = 0;
        int max = 0;
        for (int i = 0; i < nums.length; i++) {
            count += nums[i] == 0 ? -1 : 1;
            if (counts.containsKey(count)) {
                max = Math.max(max, i - counts.get(count));
            } else {
                counts.put(count, i);
            }
        }
        return max;
    }
}
```

### Count that max tweaked

Like most Java solutions we can squeeze out just a tiny increased performance
if we can avoid creating objects / boxing. Here we do just that by following
the prior approach with a switch to lowest index tracking in an array instead
of the map.

The time and space complexity remain the same.

```java
class Solution {
    public int findMaxLength(int[] nums) {
        // count -> lowest index seen at
        // in theory could be from [-n, n]
        int[] counts = new int[2 * nums.length + 1];
        Arrays.fill(counts, -2);
        // Make implicit 0 count at -1 explicit.
        counts[nums.length] = -1;
        int count = 0;
        int max = 0;
        for (int i = 0; i < nums.length; i++) {
            count += nums[i] == 0 ? -1 : 1;
            int pos = counts[count + nums.length];
            if (pos > -2) {
                max = Math.max(max, i - pos);
            } else {
                counts[count + nums.length] = i;
            }
        }
        return max;
    }
}
```
