# Subsets II

Given an integer array `nums` that may contain duplicates, return all possible
subsets (the power set). The solution set **must not** contain duplicate sets.
Return the solution in **any order**.

## Hints

1. This is just [Subsets](../subsets) with the change that `nums` may contain
   duplicate sets. Power set generation approaches should still apply.

## Solutions

### Bit mask your way to nirvana

This solution uses the bit mask approach to generating power sets. It starts
by sorting `nums` to ensure generated lists are in the same order and thus
the set accumulator will take care of rejecting duplicates.

The time complexity of this algorithm is O(2^n) as each member of the power
set must be produced. The space complexity is also O(2^n).

```java
class Solution {
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        Arrays.sort(nums);
        Set<List<Integer>> acc = new HashSet<>();
        int max = (int) Math.pow(2, nums.length);
        for (int i = 0; i < max; i++) {
            List<Integer> l = new ArrayList<>();
            for (int j = 0; j < nums.length; j++) {
                if ((i & (1 << j)) != 0) {
                    l.add(nums[j]);
                }
            }
            acc.add(l);
        }
        return new ArrayList<>(acc);
    }
}
```

### Oh you crazy cascade

Another approach to generating a power set is the "cascading" approach. With
this approach you seed a set with the empty set. Then you iterate through your
candidates. On each round you keep everything that is already there, and
additionally add:

*   a list with just the new candidate
*   a list for every existing list with the contents plus the candidate

This approach will produce the power set. By sorting the inputs and then
accumulating into a `Set`, the duplicate lists will be rejected.

The time and space complexity here are still O(2^n). This is the nature of
the problem as each element of the power set is considered / stored.

```java
class Solution {
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        Arrays.sort(nums);
        Set<List<Integer>> acc = new HashSet<>();
        acc.add(new ArrayList<>());
        for (int n : nums) {
            Set<List<Integer>> temp = new HashSet<>();
            List<Integer> single = new ArrayList<>();
            single.add(n);
            temp.add(single);
            for (List<Integer> l : acc) {
                // Add copy of existing.
                temp.add(new ArrayList<>(l));
                // Add copy of existing plus new element.
                List<Integer> additional = new ArrayList<>(l);
                additional.add(n);
                temp.add(additional);
            }
            acc = temp;
        }
        return new ArrayList<>(acc);
    }
}
```
