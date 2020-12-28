# Word Ladder

Given two words (`beginWord` and `endWord`), and a dictionary's word list, find
the length of shortest transformation sequence form `beginWord` to `endWord`,
such that:

1. Only one letter can be changed at a time.
1. Each transformed word must exist in the word list.

**Note:**

* Return 0 is theeree is no such transformation sequence.
* All words have the same length.
* All words contain only lowercase alphabetic characters.
* You may assume no duplicates in the word list.
* You may assume `beginWord` and `endWord` are non-empty and are not the same.

## Hints

Here you are looking for the shortest transform. Searching for all transforms,
keeping track of them, and choosing the shortest may work in theory, but in
leeter practice you are going to time out with such an approach. Thus, a breadth
first search (BFS) of the solution space certainly seems like the way to go.
By building up the words that can be transformed to in a BFS queue, the shortest
solution will be the first one you encounter.

## Solutions

### BFS

This solution marches from the `beginWord` to the `endWord` via a BFS queue
as suggested in the hint section. This is done by building up a map from
possible "wildcard words" to the `wordList` words that can be transitioned
to.

```java
class Solution {
    // We wrap a word in a class to additionally track the transform depth.
    private class Word {
        String value;
        int depth;

        Word(String value, int depth) {
            this.value = value;
            this.depth = depth;
        }
    }

    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        // Obviously if the endWord is not in wordList, transform is impossible.
        if (!wordList.contains(endWord)) {
            return 0;
        }

        // Map from transformed string to the strings in wordList
        // it matches. For example, this could have the entry:
        // h*t -> [hot, hat, hit]
        Map<String, List<String>> transforms = new HashMap<>();
        for (String w : wordList) {
            for (String t : transforms(w)) {
                List<String> l = transforms.get(t);
                if (l == null) {
                    l = new ArrayList<>();
                    transforms.put(t, l);
                }
                l.add(w);
            }
        }

        // Start a queue with with beginWord in it, this
        // facilitates a BFS for the best (shortest) solution.
        Deque<Word> d = new LinkedList<>();
        d.addLast(new Word(beginWord, 1));
        Set<String> visited = new HashSet<>();
        visited.add(beginWord);

        while (!d.isEmpty()) {
            Word w = d.removeFirst();
            // Are we done?
            if (w.value.equals(endWord)) {
                return w.depth;
            }

            // For each transform from the current word, add the
            // wordList items it can transition to, if it has not
            // be added before (avoid loops).
            for (String t : transforms(w.value)) {
                List<String> l = transforms.get(t);
                if (l == null) continue;
                for (String s : l) {
                    if (!visited.contains(s)) {
                        d.addLast(new Word(s, w.depth + 1));
                    }
                    visited.add(s);
                }
            }
        }

        // We emptied the queue and no solution was found.
        return 0;
    }

    private List<String> transforms(String s) {
        List<String> result = new ArrayList<>();
        for (int i = 0; i < s.length(); i++) {
            StringBuilder buf = new StringBuilder()
                .append(s.substring(0, i))
                .append("*")
                .append(s.substring(i + 1));
            result.add(buf.toString());
        }
        return result;
    }
}
```
