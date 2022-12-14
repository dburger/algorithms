# Longest Subarray of 1s After Deleting One Element

Given a binary array `nums`, you should delete one element from it.

Return the size of the longest non-empty subarray containing only `1`'s in the
resulting array. Return `0` if there is no such subarray.

## Hints

1. Think in terms of runs.

## Solutions

### Running on empty

This falls in a class of problems that can be solved by establishing a list of
"runs". Here, the `Run` class is used to track the different runs within the
array. This allows a second pass to fairly easily answer the posed problem.
There are a few cases to consider. If you come upon a `Run` of one `0` then
you can delete it and you will have a run the length of the peers on its left
and right. If you are looking at a run of `1`'s, then your max from that is
either its size (in the case where you have other runs to delete from), or its
size minus one if that is the only run from your input.

The time complexity of this solution is O(n) where `n` is the length of the
input array. This comes from taking a pass through the array to create the list
of runs and a second pass through runs itself. The space complexity is also
O(n). This comes from the fact that each element could alternate between `1`
and `0` and thus the size of `runs` would be the same as the size of `nums`.

```java
class Solution {
    class Run {
        int val;
        int count;
        Run(int val, int count) {
            this.val = val;
            this.count = count;
        }
    }

    public int longestSubarray(int[] nums) {
        List<Run> runs = new ArrayList<>();
        Run curr = new Run(nums[0], 1);
        runs.add(curr);
        for (int i = 1; i < nums.length; i++) {
            int val = nums[i];
            if (val == curr.val) {
                curr.count++;
            } else {
                curr = new Run(val, 1);
                runs.add(curr);
            }
        }

        int max = 0;
        for (int i = 0; i < runs.size(); i++) {
            Run run = runs.get(i);
            if (run.val == 0 && run.count == 1) {
                int left = i > 0 ? runs.get(i - 1).count : 0;
                int right = i < runs.size() - 1 ? runs.get(i + 1).count : 0;
                int size = left + right;
                 if (size > max) {
                    max = size;
                }
            } else if (run.val == 1) {
                int size = runs.size() > 1 ? run.count : run.count - 1;
                if (size > max) {
                    max = size;
                }
            }
        }
        return max;
    }
}
```
