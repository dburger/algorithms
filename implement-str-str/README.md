# Implement strStr()

Implement strStr().

Return the index of the first occurrence of needle in haystack, or `-1` if
`needle` is not part of `haystack`.

**Clarification:**

What should we return when `needle` is an empty string? This is a great question
to ask during an interview.

For the purposes of this problem, we will return 0 when `needle` is an empty
string. This is consistent to C's `strstr()` and Java's `indexOf()`.

## Hints

1. I don't think any interviewer really wants a KMP or BM solution here. A simple
   iteration should suffice.

## Solutions

### Simple iteration

This solution looks for a first character match and if found enters a second
loop trying to match the whole `needle`.

```java
class Solution {
    public int strStr(String haystack, String needle) {
        int nlen = needle.length();
        if (nlen == 0) {
            return 0;
        }
        int hlen = haystack.length();
        if (hlen == 0 || hlen < nlen) {
            return -1;
        }
        int max = hlen - nlen + 1;
        for (int i = 0; i < max; i++) {
            if (needle.charAt(0) == haystack.charAt(i)) {
                int j = 1;
                for (; j < nlen; j++) {
                    if (needle.charAt(j) != haystack.charAt(i + j)) {
                        break;
                    }
                }
                if (j == nlen) {
                    return i;
                }
            }
        }
        return -1;
    }
}
```
