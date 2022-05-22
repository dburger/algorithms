# Finding 3 Digit Even Numbers

You are given an integer array `digits`, where each element is a digit. The
array may contain duplicates.

You need to find **all** the **unique** integers that follow the given
requirements:

- The integer consists of the **concatenation** of **three** elements from
  `digits` in **any** arbitrary order.
- The integer does not have **leading zeros**.
- The integer is **even**.
For example, if the given `digits` were `[1, 2, 3]`, integers `132` and `312`
follow the requirements.

Return a **sorted** array of the unique integers.

## Hints

1. The solution space is limited to even numbers in `[100, 998]`. Can we mark
   these directly?

## Solutions

### Mark em and go.......

As noted in the hints, the solution space is over a range. This solution works
by setting up a boolean array over this range, and then marking the values that
can be achieved. The values that can be acheived are determined by doing a
loop over that range, and checking if we have enough digits, from a previously
determined digit count, to form that number. Finally a loop is done to collect
up the results.

The time complexity of this solution is O(1). That is the loops go up to `998`
regardless of the size of the input. Likewise, the space complexity is O(1),
as the same structures are allocated regardless of the input, with a result
size up to the maximum number of values extracted from the constricted range.

```java
class Solution {
    public int[] findEvenNumbers(int[] digits) {
        int[] digitCounts = new int[10];
        for (int d : digits) {
            digitCounts[d]++;
        }

        int count = 0;
        boolean[] matches = new boolean[999];
        for (int i = 100; i < 999; i += 2) {
            int temp = i;
            int d3 = temp % 10;
            temp /= 10;
            int d2 = temp % 10;
            temp /= 10;
            int d1 = temp;
            int num = 0;
            if (d1 == d2 && d2 == d3) {
                num = digitCounts[d1] - 2;
            } else if (d1 == d2) {
                num = (digitCounts[d1] - 1) * digitCounts[d3];
            } else if (d2 == d3) {
                num = (digitCounts[d2] - 1) * digitCounts[d1];
            } else if (d1 == d3) {
                num = (digitCounts[d1] - 1) * digitCounts[d2];
            } else {
                num = digitCounts[d1] * digitCounts[d2] * digitCounts[d3];
            }
            if (num > 0) {
                matches[i] = true;
                count++;
            }
        }

        int i = 0;
        int[] result = new int[count];
        for (int j = 100; j < 999; j += 2) {
            if (matches[j]) {
                result[i++] = j;
            }
        }

        return result;
    }
}
```
