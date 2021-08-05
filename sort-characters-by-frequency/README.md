# Sort Characters by Frequency

Given a string `s`, sort it in decreasing order based on the frequency of
characters, and return the sorted string.

## Hints

1. The obvious approach is to count and then sort by that count.

## Solutions

### Count and sort

This approach is the "obvious" approach with a couple of typical implementation
tricks for Java. We simply count the characters in a map. The keys of the map
are pulled out into a list. The list is then sorted using a comparator that
defers to the counts in the map. Finally, an iteration is performed on the
sorted keys where the number of times to emit is determined from the count
map.

The time complexity for this solution is O(n * log(n)) due to the sort. The
space complexity is O(n) in order to hold the count map.

```java
class Solution {
    public String frequencySort(String s) {
        // Count em.
        Map<Character, Integer> m = new HashMap<>();
        for (char c : s.toCharArray()) {
            m.put(c, m.getOrDefault(c, 0) + 1);
        }
        // Put in a list and sort by the counts.
        List<Character> chars = new ArrayList<>(m.keySet());
        Collections.sort(chars, (c1, c2) -> {
           return m.get(c2).compareTo(m.get(c1));
        });
        // Now build a sorted string emitting by the counts.
        StringBuilder buf = new StringBuilder();
        for (char c : chars) {
            for (int i = 0; i < m.get(c); i++) {
                buf.append(c);
            }
        }
        return buf.toString();
    }
}
```
