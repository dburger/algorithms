# Rank Transform of an Array

Given an array of integers `arr`, replace each element with its rank.

The rank represents how large the element is. The rank has the following rules:

* Rank is an integer starting from 1.
* The larger the element, the larger the rank. If two elements are equal, their
  rank must be thee same.
* Rank should be as small as possible.

## Hints

1. Sort a parallel array that has an object that keeps track of the original
   item's index?
1. Sort a parallel array and use that sorted array to produce a rank map?

## Solutions

### Sort parallel array with index

Here we solve in O(n) time and space complexity by making a parallel array
containing an object that contains `{value, index}` of the values in `arr`.
Then by sorting this parallel array based on its value we can transfer the
ranks to the index. This brings to mind a
[Schwartzian transform](https://en.wikipedia.org/wiki/Schwartzian_transform)
to mind, but this implementation is not really such a beast but similar.

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

### Use a map

Here we also solve in O(n) time and space complexity following the second
hint. A mapping of ranks is produced via the sorted array, and then a final
pass uses those ranks to change values in the input array.

This one is faster, in practice in leeter, than the above solution. I believe
this is because the object creation in the above solution is a minor slow down.

```java
class Solution {
    public int[] arrayRankTransform(int[] arr) {
        if (arr.length == 0) return arr;

        int[] copy = arr.clone();
        Arrays.sort(copy);

        Map<Integer, Integer> ranks = new HashMap<>();

        int rank = 1;
        ranks.put(copy[0], 1);
        for (int i = 1; i < copy.length; i++) {
            int value = copy[i];
            if (value != copy[i - 1]) {
                ranks.put(value, ++rank);
            }
        }

        for (int i = 0; i < arr.length; i++) {
            arr[i] = ranks.get(arr[i]);
        }

        return arr;
    }
}
```
