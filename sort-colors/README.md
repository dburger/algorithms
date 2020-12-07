# Sort Colors

Given an array `nums` with `n` objects colored red, white, or blue, sort them
in-place so that objects of the same color are adjacent, with the colors in the
order red, white, and blue.

Here, we will use the integers `0`, `1`, and `2` to represent the color red,
white, and blue respectively.

**Follow up:**

* Could you solve this problem without using the library's sort function?
* Could you come up with a one-pass algorithm using only `O(1)` constant space?

## Hints

1. A click cheat on this is to count instead of sort. Then a second pass count
   adjust the array.

## Solutions

### Count and double pass

Here we count the reds and whites on a first pass. We don't need to count
blues because they are just all the leftover. In a second pass we just
set the values according to the counts. Not sure if this is cheating per
the problem description or not.

```java
class Solution {
    public void sortColors(int[] nums) {
        int numRed = 0;
        int numWhite = 0;
        for (int i = 0; i < nums.length; i++) {
          switch (nums[i]) {
            case 0:
              numRed++;
              break;
            case 1:
              numWhite++;
              break;
          }
        }
        for (int i = 0; i < nums.length; i++) {
          if (numRed-- > 0) {
            nums[i] = 0;
          } else if (numWhite-- > 0) {
            nums[i] = 1;
          } else {
            nums[i] = 2;
          }
        }
    }
}
```

### Swap in place

I beleive this is the intended solution. here we maintain three index pointers.
The index pointers are interpreted as one beyond, or one before, where the
corresponding color is already established. The algorithm proceeds until the
white index pointer passes the blue. Comments indicate the decisions at each
step.

```java
class Solution {
    public void sortColors(int[] nums) {
        int red = 0;
        int white = 0;
        int blue = nums.length - 1;
        while (white <= blue) {
            switch (nums[white]) {
                case 0:
                    // If red, swap it to red location and increment both
                    // involved index pointers. The red would have been
                    // "pointing" at a white.
                    int temp = nums[white];
                    nums[white] = nums[red];
                    nums[red] = temp;
                    red++;
                    white++;
                    break;
                case 1:
                    // If already white, increment to look at next character on
                    // next iteration.
                    white++;
                    break;
                case 2:
                    // If blue, swap it to a blue location and continue with
                    // swapped character on next iteration.
                    temp = nums[blue];
                    nums[blue] = nums[white];
                    nums[white] = temp;
                    blue--;
                    break;
            }
        }
    }
}
```
