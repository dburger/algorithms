# Relative Sort Array

Given two arrays `arr1` and `arr2`, the elements of `arr2` are distinct, and
all elements in `arr2` are also in `arr1`.

Sort the elements of `arr1` such that the relative ordering of items in `arr1`
are the same as in `arr2`. Elements that do not appear in `arr2` should be
placed at the end of `arr1` in **ascending** order.

## Hints

1. Note the constraints on the values in the arrays. As these are limited to
   [0, 1000], some clever tricks can result in constant time solutions.

## Solutions

### Mind that range!

Typically when we think of a solution including a "sort" the best we are
going to be able to do is O(n * log(n)) for the time complexity unless we can
do something like a bucket sort. In this case the input is limited to
`[0, 1000]`. Since this range easily fits in memory we can directly bucket and
track the values.

How do we make a solution from this? Well we first set up a boolean array that
tracks what valus are seen in `arr2`. Then we do an integer array that counts
the values seen in `arr1` along with an unseen array tracking values seen in
`arr1` but not seen in `arr2`. With these structures constructed we are ready
to complete a solution. We proceed by allocating the solution array. Then we
fill it by first iterating through `arr2` and adding each value seen in `arr1`
in the counts that it was seen at. Lastly we iterate through the unseen and
add the elements in the counts they were seen at. This gives us the solution
without ever cranking a typical sorting algorithm over the inputs.

The time complexity of this solution is O(1) as regardless of the input size
we are merely doing some number of 1001 element iterations. The size complexity
is also O(1) to make this 1001 element structures regardless of input size.

```java
class Solution {
    public int[] relativeSortArray(int[] arr1, int[] arr2) {
        int[] seen1 = new int[1001];
        int[] unseen1 = new int[1001];
        boolean[] seen2 = new boolean[1001];

        for (int i : arr2) {
            seen2[i] = true;
        }

        for (int i : arr1) {
            if (seen2[i]) {
                seen1[i]++;
            } else {
                unseen1[i]++;
            }
        }

        int i = 0;
        int[] result = new int[arr1.length];

        // First load in those seen in arr2.
        for (int j : arr2) {
            int count = seen1[j];
            while (count-- > 0) {
                result[i++] = j;
            }
        }

        // Now load in those not seen in arr2.
        for (int j = 0; j < 1001; j++) {
            int count = unseen1[j];
            while (count-- > 0) {
                result[i++] = j;
            }
        }

        return result;
    }
}
```
