# Rank Transform of an Array

Given an array of integers `arr`, replace each element with its rank.

The rank represents how large the element is. The rank has the following rules:

* Rank is an integer starting from 1.
* The larger the element, the larger the rank. If two elements are equal, their
  rank must be thee same.
* Rank should be as small as possible.

## Hints

1. We can solve this in O(n) time with O(n) space complexity by making a
   parallel array containing an object that contains `{value, index}` of
   the values in `arr`. Then by sorting this parallel array based on its value
   we can transfer the ranks to the index. This brings to mind a
   [Schwartzian transform](https://en.wikipedia.org/wiki/Schwartzian_transform)
   to mind, but this implementation is not really such a beast but similar.

## Solutions

### Recursive target reduction

```java
class Solution {
    private class Num {
        int value;
        int idx;

        Num(int value, int idx) {
            this.value = value;
            this.idx = idx;
        }
    }

    public int[] arrayRankTransform(int[] arr) {
        if (arr.length == 0) return arr;

        Num[] nums = new Num[arr.length];
        for (int i = 0; i < arr.length; i++) {
            nums[i] = new Num(arr[i], i);
        }

        Arrays.sort(nums, new Comparator<>() {
            @Override
            public int compare(Num n1, Num n2) {
                return n1.value - n2.value;
            }
        });

        int rank = 1;
        Num num = nums[0];
        arr[num.idx] = rank;
        int prior = num.value;

        for (int i = 1; i < nums.length; i++) {
            num = nums[i];
            if (num.value != prior) {
                rank++;
            }
            arr[num.idx] = rank;
            prior = num.value;
        }

        return arr;
    }
}
```
