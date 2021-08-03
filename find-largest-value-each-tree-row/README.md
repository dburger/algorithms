# Find Largest Value in Each Tree Row

Given the `root` of a binary tree, return an array of the largest value in each
row of the tree (0-indexed).

## Hints

1. The BFS process tree by level works nicely for this.
1. DFS works as well, try passing down the depth as you recurse.

## Solutions

### BFS level at a time

The BFS traversal approach with nested loops to process a level at a time works
very nicely for this. Here we simply determine the max for each level and add
that to an accumlating list.

The time complexity of this solution is O(n). That is each node of the tree must
be visited to determine max values. The space complexity of this solution is
related to the size of the largest level as the queue used to process the BFS
holds the nodes of 2 levels at a time. This is easily bound by O(n).

```java
class Solution {
    public List<Integer> largestValues(TreeNode root) {
        List<Integer> l = new ArrayList<>();
        if (root == null) {
            return l;
        }
        Deque<TreeNode> d = new LinkedList<>();
        d.addFirst(root);
        int depth = 0;
        while (!d.isEmpty()) {
            int size = d.size();
            int max = d.peekFirst().val;
            for (int i = 0; i < size; i++) {
                TreeNode n = d.removeFirst();
                if (n.val > max) {
                    max = n.val;
                }
                if (n.left != null) {
                    d.addLast(n.left);
                }
                if (n.right != null) {
                    d.addLast(n.right);
                }
            }
            l.add(max);
        }
        return l;
    }
}
```

### DFS max that list

In a DFS traversal approach to this problem we pass the depth and an
accumulating list into the recursive calls. The depth is used to
index into the list to track the max value seen at each depth.

The time complexity of this solution is O(n) as each node in the tree is
visited. The space complexity corresponds to the number of levels in the
tree as they must be accounted for in the accumulating list and in the
recursive calls. Thus the time complexity is O(n).

```java
class Solution {
    public List<Integer> largestValues(TreeNode root) {
        List<Integer> l = new ArrayList<>();
        lv(root, 0, l);
        return l;
    }

    private void lv(TreeNode node, int depth, List<Integer> l) {
        if (node == null) {
            return;
        }
        if (depth > l.size() - 1) {
            l.add(node.val);
        } else {
            if (node.val > l.get(depth)) {
                l.set(depth, node.val);
            }
        }
        lv(node.left, depth + 1, l);
        lv(node.right, depth + 1, l);
    }
}
```
