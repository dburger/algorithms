# Intersection of Two Arrays

Given two integer arrays `nums1` and `nums2`, return an array of their
intersection. Each element in the result must be unique and you may return
the result in any order.

Constraints:
- `1 <= nums1.length, num2.length <= 1000`
- `0 <= nums1[i], nums2[i] <= 1000`

## Hints

1. Do the obvious for practice.
1. Use the contraints.

## Solutions

### Doing the obvious

The obvious way to do this is to just use sets. First the elements
of `nums1` are added to a set. A second set is created and during
an iteration of `nums2` the matching elements are added to the
second set. Lastly, convert the second set to an array and return
it.

The time and space complexity of this algorithm is O(n) where `n`
is the length of the longer input array. This could be improved
slightly by choosing which array to process first versus second
in the above approach. Processing the smaller array first will
reduce the space complexity to the order of the smaller array
(Note that technically with the inputs constrained to <= 1000
elements this implementation O(1)).

```java
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        Set<Integer> targets = new HashSet<Integer>();
        for (int n : nums1) {
            targets.add(n);
        }
        Set<Integer> founds = new HashSet<Integer>();
        for (int n : nums2) {
            if (targets.contains(n)) {
                founds.add(n);
            }
        }
        int[] result = new int[founds.size()];
        int i = 0;
        for (int n : founds) {
            result[i++] = n;
        }
        return result;
    }
}
```

### Exploiting constraints

The above solution is fast but as is often the case we can
exploit some of the listed constraints to make something
even more efficent. We can make the implicit O(1) explicit
in the code. Note that the entries are constrained
to `[0, 1000]`. This allows us to use simple counting array
to perform the checks.

With this approach the the loops and output never have to have
more than 1000 iterations / elements. Thus both the time and
space complexity are O(1).

```java
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        int[] targets = new int[1001];
        for (int i = 0; i < nums1.length; i++) {
            targets[nums1[i]] = 1;
        }
        int count = 0;
        for (int i = 0; i < nums2.length; i++) {
            if (targets[nums2[i]] == 1) {
                targets[nums2[i]] = 2;
                count++;
            }
        }
        int[] result = new int[count];
        int i = 0;
        for (int j = 0; j < targets.length; j++) {
            if (targets[j] == 2) {
                result[i++] = j;
            }
        }
        return result;
    }
}
```
