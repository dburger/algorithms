# Maximum Repeating Substring

For a string `sequence`, a string `word` is **k-repeating** if `word`
concatenated `k` times is a substring of `sequence`. The `word`'s
**maximum k-repeated value** is the highest value `k` where `word` is
**k-repeating** in `sequence`. If `word` is not a substring of `sequence`,
`word`'s maximum **k-repeating** value is `0`.

Given strings `sequence` and `word`, return the **maximum k-repeating value**
of `word` in `sequence`.

## Hints

1. Simple searches of increasing `word` concatenation should work.

## Solutions

### Loop with StringBuilder

This is just a straightforward translation of the problem statement into code.
Nothing fancy.

```java
class Solution {
    public int maxRepeating(String sequence, String word) {
        StringBuilder b = new StringBuilder(word);
        int k = 0;
        while (sequence.indexOf(b.toString()) > -1) {
            b.append(word);
            k++;
        }
        return k;
    }
}
```
