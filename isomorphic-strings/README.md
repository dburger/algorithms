# Isomorphic Strings

## Hints

## Solutions

### Mappings all around

In this solution we keep mappings from the `s` string to what it needs to be in
the `t` string. We also keep a `Set` that holds the characters we have already
mapped. For each character we check what it is already mapped to. If it is
already mapped for another character we are done. If not we maintain the
mappings and go on to the next character.

This solution has time complexity of O(n) where n is the length of the strings.
The space complexity is also O(n) to hold the mappings.

```java
class Solution {
    public boolean isIsomorphic(String s, String t) {
        if (s.length() != t.length()) {
            return false;
        }
        Map<Character, Character> translations = new HashMap<>();
        Set<Character> used = new HashSet<>();
        for (int i = 0; i < s.length(); i++) {
            char sc = s.charAt(i);
            char tc = t.charAt(i);
            Character mapped = translations.get(sc);
            if (mapped == null) {
                if (used.contains(tc)) {
                    // Already mapped from a different character, fail.
                    return false;
                }
                translations.put(sc, tc);
                used.add(tc);
            } else if (mapped != tc) {
                // Already mapped to a different character, fail.
                return false;
            }
        }
        return true;
    }
}
```
