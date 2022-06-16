# Minimum Swaps to Make Strings Equal

You are given two strings `s1` and `s2` of equal length consisting of letters
`"x"` and `"y"` **only**. Your task is to make these two strings equal to each
other. You can swap any two characters that belong to **different** strings,
which means: swap `s1[i]` and `s2[j]`.

## Hints

1. What kind of mismatches are there? How many swaps to fix?

## Solutions

### Count your types, match your swaps

When there are mismatches to fix, note that they can essentially come in two
types. We can form pairs of:

xx
yy

and this can be fixed in one swap. The other type is:

xy
yx

and this can be fixed in two swaps. For example first swap the top x and bottom
y, then do a diagonal swap.

Thus to get the minimum number of swaps you want to fix all you can of the first
form (one swap each) and then follow that by fixing the second form (two swaps
each).

This solution proceeds by counting the mismatches. Here the variable names
indicate lining up the strings with `s1` on "top" and `s2` on the bottom. Then
we count the mismatches with `x` on top and `y` on top. We count the number of
pairs of each of those we can fix. Then we add in the two swap fixes if the
leftovers match up.

The time complexity of this solution is O(n) where `n` is the length of the
strings. This comes from making a pass through the strings to count the
differences. The time complexity of this solution is O(1). The size needed for
the solution is constant regardless of the input.

```java
class Solution {
    public int minimumSwap(String s1, String s2) {
        int n = s1.length();
        if (s2.length() != n) {
            return -1;
        }

        int xtop = 0;
        int ytop = 0;
        for (int i = 0; i < n; i++) {
            char top = s1.charAt(i);
            char bottom = s2.charAt(i);
            if (top != bottom) {
                if (top == 'x') {
                    xtop++;
                } else {
                    ytop++;
                }
            }
        }

        // Two xtops can be fixed in one swap.
        int swaps = xtop / 2;
        // Likewise for two ytops.
        swaps += ytop / 2;

        // What wasn't fixed in one swap is leftover. It will be either
        // one or zero because pairs were handled above.
        xtop %= 2;
        ytop %= 2;

        if (xtop == ytop) {
            // If the leftover are equal, each pair can be fixed in two swaps.
            return swaps + xtop + ytop;
        } else {
            // Else this can't be fixed.
            return -1;
        }
    }
}
```
