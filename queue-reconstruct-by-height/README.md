# Queue Reconstruction by Height

You are given an array of people, `people`, which are the attributes of some
people in a queue (not necessarily in order). Each `people[i] = [hi, ki]`
represents the `ith` person of height `hi` with **exactly** `ki` other people
in front who have a height greater than or equal to `hi`.

Reconstruct and return the queue that is represented by the input array
`people`. The returned queue should be formatted as an array `queue`, where
`queue[j] = [hj, kj]` in the attributes of the `jth` person in the queue
(`queue[0]` is the person at the front of the queue).

## Hints

1. I don't have much to say here. Think about it.

## Solutions

### Find your place

This solution works by first sorting `people` into a natural ordering, that
is by `(height, # in front)`. After this we take a second pass through
`people` moving each person into the `result` array. Where do we place them?
We start with position `0` and move each person until we have incremented
to their `# in front` value or position `pos`. If we encounter a slot that
is already occupied, if its value is less than the person we are currently
working on we increment `pos` as hopping over that value did not increase
our `# in front` count.

The running time of this algorithm is O(n^2) where `n` is the length of
`people`. Note that there is a O(n * log(n)) sort that we begin with that
gets swamped out by the O(n^2) pass through the array as we place each
element. The space complexity of this solution is O(n) as we do not do
this in place but instead construct a `result` array to accumulate the
solution.

```java
class Solution {
    public int[][] reconstructQueue(int[][] people) {
        // Order by (height, front)
        Arrays.sort(people, (a, b) -> {
            int diff = a[0] - b[0];
            return diff == 0 ? a[1] - b[1] : diff;
        });

        // Initialize result marking as unoccupied.
        int[][] result = new int[people.length][2];
        for (int[] r : result) {
            r[0] = -1;
        }

        for (int[] p : people) {
            int height = p[0];
            int pos = p[1];
            for (int i = 0; i <= pos; i++) {
                int currHeight = result[i][0];
                // If curr is filled but has less height than p we stepping over
                // it does not increase our in front count. Increment pos to accomodate.
                if (currHeight != -1 && currHeight < height) {
                    pos++;
                }
            }

            result[pos] = p;
        }

        return result;
    }
}
```
