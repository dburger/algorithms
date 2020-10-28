# Longest Substring Without Repeating Characters

Given a string `s`, find the length of the **longest substring** without
repeating characters.

## Hints

1. A brute force solution could loop over the string and find the longest
   substring with no duplicates from each starting position. This would
   involve a nested loop. Each check would involve a scan looking for
   duplicates (accumulated in a Set). This makes this O(n^3).
1. A better solution would scan forward keeping track of a start of
   uniqueness and end for current. These values would be advanced while
   holding some auxiliary structure to identify duplicates and advance
   the start index.

## Solutions

### Duplicate check in map

This solution uses a map to keep track of characters seen so far. When a
duplicate is seen, characters before the prior dupe are cleared from the
map and the start position is updated accordingly.

The outer loop advances over the string on each iteration. The inner loop
to visit the prior visited characters and remove them from the map could
have to remove every character if every character is removed as a dupe.
Thus the solution could visit every character twice and is O(2n).

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int maxLen = 0;
        int start = 0;
        Map<Character, Integer> m = new HashMap<>();
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            if (m.containsKey(c)) {
                int j = m.get(c);
                for (int k = start; k <= j; k++) {
                    m.remove(s.charAt(k));
                }
                start = j + 1;
                m.put(c, i);
            } else {
                maxLen = Math.max(maxLen, i - start + 1);
                m.put(c, i);
            }
        }
        return maxLen;
    }
}
```

### Duplicate check in set

Similar to the prior solution, but instead using a set to keep the duplicates.
This code is shorter and slicker but perhaps more subtle. When a duplicate is
detected the removal continues until offering up the new character does not
present a duplicate.

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n = s.length();
        Set<Character> seen = new HashSet<>();
        int i = 0;
        int j = 0;
        int maxLen = 0;
        while (i < n && j < n) {
            if (!seen.contains(s.charAt(j))) {
                seen.add(s.charAt(j++));
                maxLen = Math.max(maxLen, j - i);
            } else {
                seen.remove(s.charAt(i++));
           }
        }
        return maxLen;
    }
}
```
