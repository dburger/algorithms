# Circular Array Loop

You are given a **circular** array `nums` of positive and negative integers. If
a number `k` at an index is positive, then move forward `k` steps. Conversely,
if it's negative (`-k`), move backwards `k` steps. Since the array is circular,
you may assume that the last element's next element is the first element, and
the first element's previous element is the last element.

Determine if there is a loop (or a cycle) in `nums`. A cycle must start and end
at the same index and the cycle's length > 1. Furthermore, movements in a cycle
must all follow a single direction. In other words, a cycle must not consist of
both forward and backward movements.

## Hints

1. A brute force approach would involve checking from each position whether a
   cycle can be formed.
1. Making the brute force slightly slicker, the slow versus fast pointer
   approach could be used to detect the cycle.

## Solutions

### Brute force

This brute force solution does a loop that attempts to find a cycle from each
starting position. In doing so it keeps track of the direction and the nodes
that have been visited before. It bails if the direction changes, or it hits
a node it has visited before that isn't the target node.

The time complexity here is O(n^2) as it tried from each array position and
could visit up to n nodes during the try. The space complexity is O(n) to hold
the visited list.

```java
class Solution {
    public boolean circularArrayLoop(int[] nums) {
        int n = nums.length;
        if (n <= 1) {
            return false;
        }

        for (int i = 0; i < n; i++) {
            int dir = nums[i];
            int pos = i;
            int hops = 0;
            Set<Integer> visited = new HashSet<>();

            while (true) {
                int step = nums[pos];
                if (dir * step < 0) {
                    // Different signs got a negative, so no cycle
                    // from i.
                    break;
                }
                pos = (pos + step) % n;
                if (pos < 0) {
                    pos += n;
                }
                hops++;
                if (pos == i) {
                    if (hops == 1) {
                        // Back at start but done in one hop, no
                        // qualifying cycle from i.
                        break;
                    }
                    // Back at start and more than one hop, winner.
                    return true;
                } else if (visited.contains(pos)) {
                    // We are back at a pos we already visited, thus
                    // we are in a cycle that doesn't get back to i.
                    break;
                }
                visited.add(pos);
            }
        }
        return false;
    }
}
```

### Slow and fast indices approach

This approach uses a slow pointer versus a fast pointer to determine the cycle.
The slow pointer moves one hop per round, while the fast pointer moves two hops
per round. If there is a cycle the fast pointer will catch up with the slow
pointer.

This approach uses a `nextIndex` method to determine the next index. It also
flags invalid cycles by returning `-1` when the next index would be invalid.
The next index is invalid if it changes the direction of the cycle or if it
cycles around the array in a single hop. Single hop solutions are outlawed
in the problem description.

This solution is the same O(n^2) as the above solution but features an O(1)
time complexity as the visited set is no longer kept. In practice it runs
quite a bit faster against the leeter test set.

```java
class Solution {
    public boolean circularArrayLoop(int[] nums) {
        int n = nums.length;
        if (n <= 1) {
            return false;
        }

        for (int i = 0; i < n; i++) {
            int dir = nums[i];
            int slow = i;
            int fast = i;

            while (true) {
                slow = nextIndex(nums, slow, dir);
                if (slow == -1) {
                    break;
                }
                fast = nextIndex(nums, fast, dir);
                if (fast == - 1) {
                    break;
                }
                fast = nextIndex(nums, fast, dir);
                if (fast == -1) {
                    break;
                }
                if (slow == fast) {
                    return true;
                }
            }
        }
        return false;
    }

    private int nextIndex(int[] nums, int pos, int dir) {
        int step = nums[pos];
        if (step * dir < 0) {
            // Switched direction, doesn't work.
            return -1;
        }
        int nextPos = (pos + step) % nums.length;
        if (nextPos < 0) {
            nextPos += nums.length;
        }
        // If nextPos == pos then cycled back in one hop, not valid.
        return nextPos == pos ? -1 : nextPos;
    }
}
```

### Fancy pants approach

Here we use the limited allowed numeric values of `[-1000, 1000]` to mark the
array as we go. We mark positions in the process of being visited (`1001`) or
already eliminated as having a cycle through them (`1002`).

Since this approach marks each position as it goes it doesn't need to revisit
chains of positions and is thus O(n) time complexity. Here we use no extra space
in the solution and get O(1) space complexity.

The solution is essentially a DFS through the circular array.

```java
class Solution {
    public boolean circularArrayLoop(int[] nums) {
        for (int i = 0; i < nums.length; i++) {
            if (cycle(nums, i, nums[i])) {
                return true;
            }
        }
        return false;
    }

    // 1001 == visited and so far ok
    // 1002 == visited and no cycle on this path
    private boolean cycle(int[] nums, int idx, int dir) {
        int step = nums[idx];
        if (step == 1001) {
            return true;
        }
        if (step == 1002) {
            // Already determined no loop through idx.
            return false;
        }
        if (dir * step < 0) {
            // Change of direction.
            return false;
        }

        // Note the double mod to handle negative mod.
        int nextIdx = ((idx + step) % nums.length + nums.length) % nums.length;
        if (nextIdx == idx) {
            // Single cycle hop, invalid.
            return false;
        }

        // Visited idx and still possible cycle.
        nums[idx] = 1001;

        if (cycle(nums, nextIdx, dir)) {
            return true;
        }

        // Visited idx and no cycle through idx.
        nums[idx] = 1002;

        return false;
    }
}
```
