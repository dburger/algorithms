# Statistics From a Large Sample

You are given a sample of integers in the range `[0, 255]`. Since the sample is
so large, it is represented by an array `counts` where `counts[k]` is the
**number of times** that k appears in the sample.

Calculate the following statistics:

*   `minimum`: The minimum element in the sample.
*   `maximum`: The maximum element in the sample.
*   `mean`: The average of the sample, calculated as the total sum of all
     elements divided by the total number of elements.
*   `median`:
    *   If the sample has an odd number of elements, the the `median` is the
        middle element once the sample is sorted.
    *   If the sample has an even number of elements, then the `median` is the
        average of the two middle elements once the sample is sorted.
*   `mode`: The number that appears the most in the sample. It is guaranteed
    to be **unique**.

Return the statistics of the sample as an array of floating-point numbers
`[minimum, maximum, mean, median, mode]`. Answers within `10^-5` of the actual
answer will be accepted.

## Hints

1. Nothing fancy - just watch for overflow in that `mean` calculation.

## Solutions

### Double pass

This solution merely uses a two pass approach where the first pass accumulates
statistics and the second pass finds the `median` when the `count` is known.
The details to watch for here are:

1. Overflow, notice the use of `long` to prevent that.
1. Finding the median is slightly tricky.

The time complexity of this solution is O(1) since the array size is bound to
256 elements. The space complexity in this solution is also O(1).

```java
class Solution {
    public double[] sampleStats(int[] count) {
        double min = 256;
        double max = -1;
        int cnt = 0;
        long total = 0;
        int modeCount = 0;
        double mode = 0.0;
        for (int i = 0; i < count.length; i++) {
            int c = count[i];
            if (c > 0) {
                if (i < min) {
                    min = i;
                }
                if (i > max) {
                    max = i;
                }
                cnt += c;
                total += c * (long) i;
                if (c > modeCount) {
                    modeCount = c;
                    mode = i;
                }
            }
        }
        double mean = total / (double) cnt;
        boolean odd = cnt % 2 == 1;
        int target = cnt / 2;
        if (odd) {
            target++;
        }
        int j = 0;
        double median = 0.0;
        for (int i = 0; i < count.length; i++) {
            j += count[i];
            // figure out if passed the median and pull it out
            if (j > target) {
                median = i;
                break;
            } else if (j == target) {
                if (odd) {
                    median = i;
                    break;
                } else {
                    int k = i + 1;
                    while (count[k] == 0) {
                        k++;
                    }
                    median = (i + k) / 2.0;
                    break;
                }
            }
        }
        return new double[] {min, max, mean, median, mode};
    }
}
```
