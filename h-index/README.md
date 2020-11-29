# H Index

Given an array of citations (each citation is a non-negative integer) of a
researcher, write a function to compute the researcher's h-index.

From wikipedia: "A scientist has index h if h of his/her N papers have
**at least** h citations each, and the other N-h papers have
**no more than** h citations each"

## Hints

## Solutions

1. Would sorting help?

### Sort and iterate

Here we sort and for each position check if its citation count
is greater than the number of positions >= to its position in
the array. The first one of these found, is the h-index.

```java
class Solution {
    public int hIndex(int[] citations) {
        if (citations.length == 0) {
            return 0;
        }
        Arrays.sort(citations);
        int n = citations.length;
        for (int i = 0; i < n; i++) {
            if (citations[i] >= n - i) {
                return n - i;
            }
        }
        return 0;
    }
}
```
