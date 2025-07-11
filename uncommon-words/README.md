# Uncommon Words from Two Sentences

## Hints

1. This is a trivial warm up style problem solved by counting.

## Solutions

### Count me in

Here we just establish a map that maps words to the count for that word.
Obviously when we finish any count that remains at 1 was in only 1 of
the sentences.

```java
class Solution {
    public String[] uncommonFromSentences(String s1, String s2) {
        Map<String, Integer> counts = new HashMap<>();
        for (String word : s1.split(" ")) {
            counts.put(word, counts.getOrDefault(word, 0) + 1);
        }
        for (String word : s2.split(" ")) {
            counts.put(word, counts.getOrDefault(word, 0) + 1);
        }
        List<String> result = new ArrayList<>();
        for (var entry : counts.entrySet()) {
            if (entry.getValue() == 1) {
                result.add(entry.getKey());
            }
        }
        return result.toArray(new String[0]);
    }
}
```
