# Reverse String II

Given a string `s` and an integer `k`, reverse the first `k` characters for
every `2k` characters counting from the start of the string.

If there are fewer than `k` characters left, reverse all of them. If there are
less than `2k` but greater than or equal to `k` characters, then reverse the
first `k` characters and leave the other as original.

## Hints

1. Reverse chunks, jump by chunks?

## Solutions

### Reverse my array everday

The easiest way to do this problem is to translate the input string into an
array and work on that. A loop is used to jump across the input in `2k` chunks
with the first `k` being reversed in each iteration.

The time complexity of this solution is O(n) where `n` is the length of input
string `s`. The space complexity of this solution is O(1).

```java
class Solution {
    public String reverseStr(String s, int k) {
        char[] arr = s.toCharArray();
        for (int i = 0; i < arr.length; i += 2 * k) {
            reverse (arr, i, k);
        }
        return new String(arr);
    }

    private void reverse(char[] arr, int start, int num) {
        int end = Math.min(arr.length - 1, start + num - 1);
        while (start < end) {
            char temp = arr[start];
            arr[start] = arr[end];
            arr[end] = temp;
            start++;
            end--;
        }
    }
}
```
