# Three Sum

Given an array `nums` of n integers, are there elements a, b, c in `nums` such
that a + b + c = 0? Find all unique triplets in the array which gives the sum
of zero.

Notice that the solution set must not contain duplicate triplets.

## Hints

1. If the array is first sorted an iteration through the array with a sliding
   window over its tail can be used to locate solutions.

## Solutions

### Sliding Window

This solution is O(n^2) with an outer loop of almost n iterations and an inner
while loop that can cover up to almost n locations within each outer loop.

```java
class Solution {

    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);
        Set<List<Integer>> s = new HashSet<>();
        for (int i = 0; i < nums.length - 2; i++) {
            int j = i + 1;
            int k = nums.length - 1;
            while (j < k) {
                int sum = nums[i] + nums[j] + nums[k];
                if (sum == 0) {
                    s.add(Arrays.asList(nums[i], nums[j++], nums[k--]));
                } else if (sum < 0) {
                    j++;
                } else {
                    k--;
                }
            }
        }
        return new ArrayList<>(s);
    }
}
```
