# Quick Sort

Write a quick sort.

Quick sort works by recursively partitioning the input around
a chosen element.

## Hints

## Solutions

### Quick Sort

Quick sort is O(n*log(n)) and is done here in place.

```java
class Solution {
    public void qsort(int[] input) {
        qsort(input, 0, input.length - 1);
    }

    private void qsort(int[] input, int lo, int hi) {
        if (lo < hi) {
            int pivot = partition(input, lo, hi);
            qsort(input, lo, pivot -1);
            qsort(input, pivot + 1, hi);
        }
    }

    private int partition(int[] input, int lo, int hi) {
        // Just using the element at hi as the pivot.
        int pval = input[hi];
        int fill = lo;
        for (int i = lo; i < hi; i++) {
            if (input[i] < pval) {
                swap(input, i, fill++);
            }
        }
        swap(input, fill, hi);
        return fill;
    }

    private void swap(int[] input, int i, int j) {
        int temp = input[j];
        input[j] = input[i];
        input[i] = temp;
    }
}
```
