# Merge Sort

Write a merge sort. For bonus points write a version that does an in place sort.

Merge sort works by recursively splitting the input in half until it is down to
single element arrays. Then, merging of sorted arrays is used to arrive at the
solution.

## Hints

## Solutions

### Merge into new array

This one merges into a separate array, using extra space, but keeping the
big O at O(n*log(n)).

```java
class Solution {
    public void mergesort(int[] input) {
        mergesort(input, 0, input.length - 1);
    }

    private void mergesort(int[] input, int lo, int hi) {
        if (lo < hi) {
            int mid = lo + (hi - lo) / 2;
            mergesort(input, lo, mid);
            mergesort(input, mid + 1, hi);
            merge(input, lo, mid, hi);
        }
    }

    private void merge(int[] input, int lo, int mid, int hi) {
        if (lo < hi) {
            int fill = 0;
            int[] sorted = new int[hi - lo + 1];
            int lp = lo;
            int hp = mid + 1;
            // Do sorted merge into sorted.
            while (lp <= mid || hp <= hi) {
                if (lp <= mid && hp <= hi) {
                    if (input[lp] <= input[hp]) {
                        sorted[fill++] = input[lp++];
                    } else {
                        sorted[fill++] = input[hp++];
                    }
                } else if (lp <= mid) {
                    sorted[fill++] = input[lp++];
                } else {
                    sorted[fill++] = input[hp++];
                }
            }
            // Copy back.
            for (int i = 0; i < sorted.length; i++) {
                input[lo + i] = sorted[i];
            }
        }
    }
}
```
### Merge in place

Like the previous solution, but does the merge in place so that no extra space
is required. Note that this in place shift makes this sort slower at O(n^2).

```java
class Solution {
    public void mergesort(int[] input) {
        mergesort(input, 0, input.length - 1);
    }

    private void mergesort(int[] input, int lo, int hi) {
        if (lo < hi) {
            int mid = lo + (hi - lo) / 2;
            mergesort(input, lo, mid);
            mergesort(input, mid + 1, hi);
            merge(input, lo, mid, hi);
        }
    }

    private void merge(int[] input, int lo, int mid, int hi) {
        int lp = lo;
        int hp = mid + 1;
        while (lp <= mid && hp <= hi) {
            if (input[lp] <= input[hp]) {
                lp++;
            } else {
                // shift the elements out of the way
                int temp = input[hp];
                for (int i = hp; i > lp; i--) {
                    input[i] = input[i - 1];
                }
                input[lp] = temp;
                lp++;
                mid++;
                hp++;
            }
        }
    }
}
```
