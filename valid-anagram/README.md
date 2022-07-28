# Valid Anagram

Given two strings `s` and `t`, return `true` if `t` is an anagram of `s`, and
`false` otherwise.

An **anagram** is a word or phrase formed by rearranging the letters of a
different word or phrase, typically using all the original letters exactly
once.

## Hints

1. This is an easy, don't get queasy.

## Solutions

### Count yo chars

There are many ways to do this problem. Amongst the worst would be to plop the
values into character arrays, sort the arrays, and then make sure they are the
same. This would of course come with the punishment of the sort starting you
with an O(n * log(n)) time complexity. Thus counting based approaches are
preferred.

One way to do a counting based approach would be to map the characters seen to
the count seen. Then checking would be making sure both strings had the same
character counts. We can get this simpler and a bit faster in practice though
because of the constraint placed on the allowable characters. Because the
problem states that the characters are limited to lower case english characters
we can just do our counts in an array.

Wow this got wordy for an "easy". The solution here proceeds by counting for
`s` and then subtracting counts for `t`. If we go below `0` for any count
we know that we do not have an anagram.

The time complexity for this problem is O(n) where `n` is the length of the
input strings. The space complexity is O(1), as we make the same counting
array regardless of the size of the input.

```java
class Solution {
    public boolean isAnagram(String s, String t) {
        if (s.length() != t.length()) {
            return false;
        }

        int[] counts = new int[26];

        // Store the counts for s.
        for (char c : s.toCharArray()) {
            counts[c - 'a']++;
        }

        // Subtract the counts for t, if we go negative, this
        // is not an anagram.
        for (char c : t.toCharArray()) {
            if (--counts[c - 'a'] < 0) {
                return false;
            }
        }

        return true;
    }
}
```
