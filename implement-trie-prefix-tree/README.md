# Implement Trie (Prefix Tree)

A trie (pronounced as "try") or **prefix tree** is a tree data structure used
to efficiently store and retrieve keys in a datset of strings. There are
various applications of this data structure, such as autocomplete and
spellchecker.

Implement the trie class:

*   `Trie()` initializes the trie object.
*   `void insert(String word)` inserts the string `word` into the trie.
*   `boolean search(String word)` returns `true` if the string `word` is in the
    trie (i.e., was inserted before), and `false` otherwise.
*   `boolean startsWith(String prefix)` returns `true` if there is a previously
    inserted string `word` that has the prefix `prefix`, and `false` otherwise.

## Hints

1. If you've seen one before you know what to do. If not...you need layers of
   nodes that link to deeper layers. The starter here is that 26 links per
   node are needed as we are limited to "lower case english characters."

## Solutions

### Nodes and nodes and terminal nodes

This would seem to be the typical approach to implementing a string prefix
trie structure in java. A `Node` is a class that features a `terminal` flag
and array of links to 26 other nodes. The `terminal` flag indicates if the
node represents a terminal position for a full word. The links are the
connectors to the next character in the word. When null, such an outbound
link / next character does not exits. In this way we can walk down the links
to insert and traverse words. It is pretty simple if you get the data structure
right, the code flows quite easily. Note that things get trickier when you
add delete to the mix.

```java
class Trie {
    private class Node {
        boolean terminal = false;
        Node[] nodes = new Node[26];
    }

    private final Node root = new Node();

    /** Initialize your data structure here. */
    public Trie() {
    }

    /** Inserts a word into the trie. */
    public void insert(String word) {
        Node curr = root;
        for (char c : word.toCharArray()) {
            int pos = c - 'a';
            Node n = curr.nodes[pos];
            if (n == null) {
                n = new Node();
                curr.nodes[pos] = n;
            }
            curr = n;
        }
        curr.terminal = true;
    }

    /** Returns if the word is in the trie. */
    public boolean search(String word) {
        Node curr = root;
        for (char c : word.toCharArray()) {
            Node n = curr.nodes[c - 'a'];
            if (n == null) {
                return false;
            }
            curr = n;
        }
        return curr.terminal;
    }

    /** Returns if there is any word in the trie that starts with the given prefix. */
    public boolean startsWith(String prefix) {
        Node curr = root;
        for (char c : prefix.toCharArray()) {
            Node n = curr.nodes[c - 'a'];
            if (n == null) {
                return false;
            }
            curr = n;
        }
        return true;
    }
}
```
