# Delete nodes and Return Forest

Given the `root` of a binary tree, each node in the tree has a distinct value.

After deleting all nodes with a value in `to_delete`, we are left with a forest
(a disjoint union of trees).

Return the roots of the trees in the remaining forest. You may return the result
in any order.

## Hints

1. Traversal with accumulation. You need some extra information in the
   traversal. What is it?

## Solutions

### Just traverse, bring your luggage

This is a neat little problem and I don't say that because I beat 100% of the
solutions of my first submittal. As suggested in the hint, it can be done with
a simple traversal with some extra information brought along for the ride.
What is that information? Well obviously you need to track which nodes are in
the delete set. The not quite as obvious:

- Need to be able to chop of the tree from the parent
- Need to know if you are needing to add as a root

These are handled with the `parent` and `rooted` parameters below. `rooted`
refers to whether or not the node currently being looked out is connected
to a parent identified as a root. This determines if you would need to add
the current node to `roots` or not.

The time complexity of this solution is O(n) where `n` is the number of nodes
in the tree. The space complexity is also O(n) as the recursive stack frames
may need to hold the entirety of the tree.

```java
class Solution {
    public List<TreeNode> delNodes(TreeNode root, int[] to_delete) {
        List<TreeNode> roots = new ArrayList<>();
        Set<Integer> deletes = new HashSet<>();
        for (int i : to_delete) {
            deletes.add(i);
        }
        delNodes(root, null, false, deletes, roots);
        return roots;
    }

    private void delNodes(TreeNode node, TreeNode parent, boolean rooted, Set<Integer> deletes, List<TreeNode> roots) {
        if (node == null) {
            return;
        }
        if (deletes.contains(node.val)) {
            if (parent != null) {
                if (parent.left == node) {
                    parent.left = null;
                } else {
                    parent.right = null;
                }
            }
            rooted = false;
        } else if (!rooted) {
            roots.add(node);
            rooted = true;
        }
        delNodes(node.left, node, rooted, deletes, roots);
        delNodes(node.right, node, rooted, deletes, roots);
    }
}
```
