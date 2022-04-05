# Duplicate Zeros

Given a fixed-length integer array `arr`, duplicate each occurence of zero,
shifting the remaining elements to the right.

**Note** that elements beyond the length of the origin array are not written.
Do the above modifications to the input array in place and do not return
anything.

## Hints

1. By "in place" I take it that we are not allowed to copy any of the contents
   of the input array to another data structure. This changes the problem a
   lot. Can a single pass work?

## Solutions

I will provide two invalid solutions before getting on to the solution that
follows the "in place" constraint.

### Ignoring "in place", parallel race

If we ignore the "in place" constraint we can just create the result in a
parallel array, and then copy over the input array when we have exhausted the
space.

The time complexity for this solution is O(n) where n is the size of the input
array. This comes from the two passes. One pass to copy out into `copy`, and
a second pass from copying back over `arr`. The space complexity is also O(n),
as we have a structure that needs to be sized according to the input size.

```java
class Solution {
    public void duplicateZeros(int[] arr) {
        int[] copy = new int[arr.length];
        for (int i = 0, j = 0; j < arr.length; i++) {
            if (arr[i] == 0) {
                copy[j++] = 0;
                if (j < arr.length) {
                    copy[j++] = 0;
                }
            } else {
                copy[j++] = arr[i];
            }
        }
        System.arraycopy(copy, 0, arr, 0, copy.length);
    }
}
```

### Ignoring "in place", build space

Second verse, same as the first, with a list doing the dirty work. The time and
space complexities apply as before. In practice, the boxing and unboxing of
the ints makes this slightly slower than the prior solution.

```java
class Solution {
    public void duplicateZeros(int[] arr) {
        List<Integer> copy = new ArrayList<>();
        for (int i = 0; i < arr.length && copy.size() < arr.length; i++) {
            int val = arr[i];
            copy.add(val);
            if (val == 0) {
                copy.add(val);
            }
        }
        for (int i = 0; i < arr.length; i++) {
            arr[i] = copy.get(i);
        }
    }
}
```

### Passing twice is really nice.

OK, let's solve this problem per the stated constraints. Must be "in place."
How can we do this? We can't merely advance through the array while copying
out in front of ourselves when a `0` occurs. If we did that, we would have to
have an auxillary data structure to hold the values that are being clobbered.
This runs counter to the "in place" constraint. Instead, we do two passes. The
first pass is used to produce a count of the number of zeros in `arr`. A
second pass iterates over `arr` in reverse. For each element, we determine its
translated position by looking at its value and the number of zeros that are
before it.

The time complexity for this solution is O(n) where n is the length of `arr`.
This comes from doing two passes through the array. The first one to count the
zeros, and the second to move the elements. The space complexity for this
algorithm is O(1). This comes from doing it "in place".

```java
class Solution {
    public void duplicateZeros(int[] arr) {
        int zeros = 0;
        for (int i = 0; i < arr.length; i++) {
            if (arr[i] == 0) {
                zeros++;
            }
        }
        for (int i = arr.length - 1; i > -1; i--) {
            int val = arr[i];
            if (val == 0) {
                int pos = Math.max(zeros - 1, 0) + i;
                if (pos < arr.length) {
                    arr[pos] = 0;
                    pos++;
                    if (pos < arr.length) {
                        arr[pos] = 0;
                    }
                }
                zeros--;
            } else {
                int pos = zeros + i;
                if (pos < arr.length) {
                    arr[pos] = val;
                }
            }
        }
    }
}
```
