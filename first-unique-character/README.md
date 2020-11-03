# First Unique Character in a String

Given a string, find the first non-repeating character in it and return its
index. If it doesn't exist, return -1;

**Note**: You may assume the string contains only lowercase English letters.

## Hints

1. A brute force solution could do nested loops to find the first character
   in the string that doesn't have a second matching character. This would
   be O(n^2). This is dead simple to program but is not a good solution.
1. Given the note, you could use a data structure to mark seen characters?

## Solutions

### Mark in one pass, check in another

This simple solution is O(n). An array can easily be used for marking since
the note limits us to 26 characters.

```java
class Solution {
    public int firstUniqChar(String s) {
        int[] counts = new int[26];
        for (int i = 0; i < s.length(); i++) {
            // Mark.
            counts[s.charAt(i) - 'a']++;
        }
        for (int i = 0; i < s.length(); i++) {
            // Check.
            if (counts[s.charAt(i) - 'a'] == 1) {
                return i;
            }
        }
        return -1;
    }
}
```
