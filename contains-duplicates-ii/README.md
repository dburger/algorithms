# Contains Duplicates II

Given an integer array `nums` and an integer `k`, return `true` if there are
two **distinct indices** `i` and `j` in the array such that
`nums[i] == nums[j]` and `abs(i - j) <= k`.

## Hints

1. Keep track of other indices seen for a value and perform the distance check.

## Solutions

### Brute force

This is a brute force translation of the problem. The time complexity is
O(n^2) and it times out in the leeter grader.

```java
class Solution {
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        for (int i = 0; i < nums.length; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                if (nums[i] == nums[j] && Math.abs(i - j) <= k) {
                    return true;
                }
            }
        }
        return false;
    }
}
```

### Brute force slightly improved

Here we take the above brute force solution and make it every so slightly
less brutish. The check for the absolute value difference is moved into the
nested loop condition. This prevents checking for a bunch of combinations only
to reject them for being too far apart.

With this improvement the time complexity is reduced to O(n * k) and the
leeter grader is barely passed. The space complexity is O(1).

```java
class Solution {
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        for (int i = 0; i < nums.length; i++) {
            for (int j = i + 1; Math.abs(i - j) <= k && j < nums.length; j++) {
                if (nums[i] == nums[j]) {
                    return true;
                }
            }
        }
        return false;
    }
}
```

### A bit fancier with a map

Here we are still pretty brutish but we use a map to only check indices for
equivalent elements. This improves the performance in practice but relatively
to other solutions it is still pretty crappy.

```java
class Solution {
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        Map<Integer, Set<Integer>> m = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            int val = nums[i];
            Set<Integer> s = m.computeIfAbsent(val, key -> new HashSet<>());
            for (int j : s) {
                if (Math.abs(i - j) <= k) {
                    return true;
                }
            }
            s.add(i);
        }
        return false;
    }
}
```

### Make it linear, only remember one

Here we evolve the previous solution with a key insight. The insight is that
we don't need to keep the list of all the indices occupied by a value. Instead
if we only keep the index of the most recently encontered one as it has to
be the closest to what we are looking at "now." This allows us to make only
one comparison for a given duplicate encountered. If it fails the distance
check, we can replace the closest index.

This improvement makes the time complexity O(n). The space complexity is also
O(n) to hold the map.

```java
class Solution {
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        Map<Integer, Integer> m = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            int val = nums[i];
            if (m.containsKey(val)) {
                if (i - m.get(val) <= k) {
                    return true;
                }
            }
            m.put(val, i);
        }
        return false;
    }
}
```
