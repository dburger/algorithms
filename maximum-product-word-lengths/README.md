# Maximum Product of Word Lengths

Given a string array `words`, return the maximum value of
`length(word[i]) * length(word[j])` where the two words do not share common
letters. If no such two words exist return `0`.

## Hints

1. Words consist of onlly lowercase English letters. You can you map words
   to set bits and thus whether two words share a letter can be quickly
   computed.

## Solutions

### Bits using ints

This solution follows the hint. The first thing that is done is an array of
`int`s is created, one for each word. This `int`s are used to map a `1` bit
for each letter that the word contains. Thus positions `0` through `25` may
contain `1` bits. This is followed by a second loop that compares each word
to every other word. In this loop the `letters` bits are compared checking
for intersection. If there is no intersection a length producted is computed
for these two words. It is recorded as the new `max` if it is indeed
larger.

The time complexity of this algorithm is O(n^2). This comes from comparing
every word to every other word. The space complexity of this algorithm is
O(n). This is because we need the `letters` array to hold character to bit
mappings that is the size of the input `words`.

```java
class Solution {
    public int maxProduct(String[] words) {
        int[] letters = new int[words.length];
        for (int i = 0; i < words.length; i++) {
            for (char c : words[i].toCharArray()) {
                letters[i] |= (1 << (c - 'a'));
            }
        }
        int max = 0;
        for (int i = 0; i < words.length; i++) {
            for (int j = i + 1; j < words.length; j++) {
                if ((letters[i] & letters[j]) == 0) {
                    int m = words[i].length() * words[j].length();
                    if (m > max) {
                        max = m;
                    }
                }
            }
        }
        return max;
    }
}
```

### Bits using BitSets

This is the same solution as the above but uses `BitSet` instead of `int` to
manage the intersection check. While the same time and space complexity of
the above, in practice it is slightly slower. This is likely because the
more friendly `BitSet` objects and more readable code have a slight performance
penalty.

```java
class Solution {
    public int maxProduct(String[] words) {
        BitSet[] b = new BitSet[words.length];
        for (int i = 0; i < words.length; i++) {
            b[i] = new BitSet(26);
            for (char c : words[i].toCharArray()) {
                b[i].set(c - 'a');
            }
        }
        int max = 0;
        for (int i = 0; i < words.length; i++) {
            for (int j = i + 1; j < words.length; j++) {
                if (!b[i].intersects(b[j])) {
                    int m = words[i].length() * words[j].length();
                    if (m > max) {
                        max = m;
                    }
                }
            }
        }
        return max;
    }
}
```
