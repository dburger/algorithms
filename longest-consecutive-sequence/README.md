# Longest Consecutive Sequence

Given an unsorted array of integers `nums`, return the length of the longest
consecutive elements sequence.

You must write an algorithm that runs in `O(n)` time.

## Hints

1. To meet the `O(n)` time you can't do the obvious "improvement" to the
   situation by starting with a sort. What could sets do for you?

## Solutions

### Set insertion and expand

This solution works by inserting the values into a `Set`. This will give O(1)
lookup. Then the algorithm proceeds by iterating through each number and
expanding out to the largest run from that number. Numbers are removed as they
are processed. This means that the run is consumed when the first number
contained in the run is encountered and the other numbers in the run won't
cause any further expansion.

The time complexity of this algorithm is O(n). This comes from up to three
passes through the input array. The first is to populate `s`. The second is
the iteration through the input again to check each members max sequence.
Internally this loop also may iterate through the list but with a catch. Since
it removes the elements that are involved in the sequence this internal loop
in aggregate can only perfrom one additional iteration though the sequence. The
space complexity of this algorithm is also O(n), obviously to hold the input
in sets.

```java
class Solution {
    public int longestConsecutive(int[] nums) {
        Set<Integer> values = new HashSet<>();
        for (int n : nums) {
            values.add(n);
        }
        int max = 0;
        for (int n : nums) {
            if (!values.remove(n)) {
                // Largest run involved in already processed.
                continue;
            }

            int count = 1;
            int i = n - 1;
            while (values.remove(i)) {
                count++;
                i--;
            }
            i = n + 1;
            while (values.remove(i)) {
                count++;
                i++;
            }
            if (count > max) {
                max = count;
            }
            if (max >= values.size()) {
                // What is left can't beat max.
                break;
            }
        }
        return max;
    }
}
```
