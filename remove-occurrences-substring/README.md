# Remove All Occurrences of a Substring

Given two strings `s` and `part`, perform the following operation on `s` until
**all** occurrences of the substring `part` are removed:

*   Find the **leftmost** occurrence of the substring `part` and **remove**
    it from `s`.

Return `s` after removing all occurences of `part`.

## Hints

1. Brute force is dead simple.

## Solutions

The solutions I use here don't seem to warrant the medium leeter
classification.

### Brute me

Using string search methods and piecing the string back together around the
removed segment works. Note that the subsequent search after a hit searches
from `i - part.length() + 1`. That is the pieced together string could be
joining the very last character from an otherwise matched prefix to that
prefix. We can skip everything before that.

```java
class Solution {
    public String removeOccurrences(String s, String part) {
        int i = s.indexOf(part);
        while (i > -1) {
            s = s.substring(0, i) + s.substring(i + part.length());
            i = s.indexOf(part, i - part.length() + 1);
        }
        return s;
    }
}
```

### Brute me with tweaks

We can tweak the prior solution to avoid the expense of creating new strings
and instead manipulate a mutable `StringBuilder`. This squeezes out a little
more performance from this algorithm.

```java
class Solution {
    public String removeOccurrences(String s, String part) {
        StringBuilder sb = new StringBuilder(s);
        int i = sb.indexOf(part);
        while (i > -1) {
            sb.delete(i, i + part.length());
            i = sb.indexOf(part, i - part.length() + 1);
        }
        return sb.toString();
    }
}
```
