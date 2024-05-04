# Longest Strictly Increasing or Strictly Decreasing Subarray

You are given an array of integers nums. Return the length of the longest
subarray of nums which is either strictly increasing or strictly
decreasing.

## Hints

1. Keep track of the current state and transition as necessary.

## Solutions

### Three States to Contemplate

This solution proceeds by merely keeping track of the three
possible states and the length of the current run. Those states
are currently increasing, currently decreasing, or currently
equivalent to last. Transitions to a new state check if they
might happen to be the new longest. The final trick is to make
sure you check if the final run won the contest. The big O running
time of this algorithm is O(n), where n is the length of the input.
This comes from the need to look at every element. The big O space
complexity of this algorithm is O(1). Only a constant amount of state
tracking variables are required regardless of the input size.

```java
class Solution {
    public int longestMonotonicSubarray(int[] nums) {
        if (nums.length == 0) {
            return 0;
        }
        int run = 1;
        int longest = 1;
        int last = nums[0];
        int dir = 0;
        for (int i = 1; i < nums.length; i++) {
            int curr = nums[i];
            if (curr > last) {
                if (dir == 1) {
                    run++;
                } else {
                    if (run > longest) {
                        longest = run;
                    }
                    run = 2;
                    dir = 1;
                }
            } else if (curr < last) {
                if (dir == -1) {
                    run++;
                } else {
                    if (run > longest) {
                        longest = run;
                    }
                    run = 2;
                    dir = -1;
                }
             } else {
                if (run > longest) {
                    longest = run;
                }
                run = 0;
                dir = 0;
            }
            last = curr;
        }
        return Math.max(run, longest);
    }
}
```

```go
func longestMonotonicSubarray(nums []int) int {
    if len(nums) == 0 {
        return 0
    }
    run := 1
    longest := 1
    last := nums[0]
    dir := 0
    for i := 1; i < len(nums); i++ {
        curr := nums[i]
        if curr > last {
            if dir == 1 {
                run++
            } else {
                if run > longest {
                    longest = run
                }
                run = 2
                dir = 1
            }
        } else if curr < last {
            if dir == -1 {
                run++
            } else {
                if run > longest {
                    longest = run
                }
                run = 2
                dir = -1
            }
        } else {
            if run > longest {
                longest = run
            }
            run = 1
            dir = 0
        }
        last = curr
    }
    return max(longest, run)
}
```

```javascript
var longestMonotonicSubarray = function(nums) {
    if (nums.length === 0) {
        return 0;
    }
    let run = 1;
    let longest = 1;
    let last = nums[0];
    let dir = 0;
    for (let i = 1; i < nums.length; i++) {
        const curr = nums[i];
        if (curr > last) {
            if (dir === 1) {
                run++;
            } else {
                if (run > longest) {
                    longest = run;
                }
                run = 2;
                dir = 1;
            }
        } else if (curr < last) {
            if (dir === -1) {
                run++;
            } else {
                if (run > longest) {
                    longest = run;
                }
                dir = -1;
                run = 2;
            }
        } else {
            if (run > longest) {
                longest = run;
            }
            run = 1;
            dir = 0;
        }
        last = curr;
    }
    return longest > run ? longest : run;
};
```
