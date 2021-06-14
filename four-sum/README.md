# Four Sum

Given an array of `nums` of `n` integers, return an array of all the
**unique** quadruplets `nums[a], nums[b], nums[c], nums[d]` such that:

*   `0 <= a, b, c, d < n`
*   `a`, `b`, `c`, and `d` are **disinct**
*   `nums[a] + nums[b] + nums[c] + nums[d] == target`

You may return the answer in **any order**

## Hints

1. The backtracking approach seen in many of the
   [Permutations](../permutations) problems will solve the problem.
1. Sorting the input can help. This can allow a solution to be built that
   narrows down on a solution by determining if you need to increase or
   reduce a current sum.

## Solutions

### Backtracking sort skip

As noted in the hints above, the permutations backtracking approach solves the
problem. Here we follow that approach with a necessary twist. We sort the
input array. This of courses places repeats of the same digit in a
contiguous block. This allows us to skip over the consideration of the same
digit within the same output position. This prevents adding duplicate answers
into the output as required by the problem constraints.

```java
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        // Sort the input so duplicates can be rejected below.
        Arrays.sort(nums);
        List<List<Integer>> acc = new ArrayList<>();
        fs(nums, target, 0, new ArrayList<>(), acc);
        return acc;
     }

    private static void fs(int[] nums, int target, int index, List<Integer> path, List<List<Integer>> acc) {
        if (path.size() == 4) {
            if (target == 0) {
                acc.add(new ArrayList<>(path));
            }
            return;
        }
        for (int i = index; i < nums.length; i++) {
            int val = nums[i];
            path.add(val);
            fs(nums, target - val, i + 1, path, acc);
            path.remove(path.size() - 1);
            // Don't consider this value again at that index. This prevents
            // duplicate answers in the output.
            while (i + 1 < nums.length && nums[i + 1] == val) {
                i++;
            }
        }
    }
}
```

## Backtracking set reject

Here we use a similar approach to the above, but we reject duplicates by
letting a `Set` do that for us instead of skipping over digits. Note that the
input must still be sorted for the proposed solution lists to be seen as
equal. Also, the accumlator, because we use a `Set`, must be converted back
to a `List` before it is returned. Interestingly this finishes with similar
completion times to the prior. I thought it would be slower. I think the
reason is the test inputs don't exercise the duplicate results problem
aggressively enough.

```java
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        Arrays.sort(nums);
        Set<List<Integer>> acc = new HashSet<>();
        fs(nums, target, 0, new ArrayList<>(), acc);
        return new ArrayList<>(acc);
    }

    private static void fs(int[] nums, int target, int index, List<Integer> path, Set<List<Integer>> acc) {
        if (path.size() == 4) {
            if (target == 0) {
                acc.add(new ArrayList<>(path));
            }
            return;
        }
        for (int i = index; i < nums.length; i++) {
            int val = nums[i];
            path.add(val);
            fs(nums, target - val, i + 1, path, acc);
            path.remove(path.size() - 1);
        }
    }
}
```

## Narrow that window

Now we jump to a different style of approach. This approach starts
with picking the first two of the values. Then it exploits the sorting
of the inputs to narrow in on the final two values that will work with
the starting two values. Duplicates are rejected by entering the solutions
into a `Set`.

Note that you could make the loop comparisons be `i < nums.length - 3` and
`j < nums.length - 2`. This leaves enough room for `left` and `right` to
always have at least one set of valid values. Here we didn't bother as the
`left < right` comparisons reject invalid starting points automatically,
without the need of further cluttering the for conditions. The change, while
simple, does not change the Big O time complexity.

```java
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        Set<List<Integer>> acc = new HashSet<>();
        Arrays.sort(nums);
        for (int i = 0; i < nums.length; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                int ival = nums[i];
                int jval = nums[j];
                int base = ival + jval;
                int left = j + 1;
                int right = nums.length - 1;
                while (left < right) {
                    int lval = nums[left];
                    int rval = nums[right];
                    int sum = base + lval + rval;
                    if (sum < target) {
                        left++;
                    } else if (sum > target) {
                        right--;
                    } else {
                        acc.add(List.of(ival, jval, lval, rval));
                        left++;
                        right--;
                    }
                }
            }
        }
        return new ArrayList<>(acc);
    }
}
```
