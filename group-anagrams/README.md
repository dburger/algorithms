# Group Anagrams

Given an array of strings `strs`, group **the anagrams** together. You can
return the answer in **any order**.

An anagram is a word or phrase formed by rearranging the letters of a different
word or phrase, typically using all the original letters exactly once.

## Hints

1. How can you get a canonical representation of each string.

## Solutions

### Multimap by canonical representation

```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<String, List<String>> m = new HashMap<>();
        for (String s : strs) {
            char[] chars = s.toCharArray();
            Arrays.sort(chars);
            String canonical = String.valueOf(chars);
            List<String> group = m.get(canonical);
            if (group == null) {
                group = new ArrayList<>();
                m.put(canonical, group);
            }
            group.add(s);
        }
        return new ArrayList<>(m.values());
    }
}
```
