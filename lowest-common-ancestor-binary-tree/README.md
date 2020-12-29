# Lowest Common Ancestor of a Binary Tree

Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in
the tree.

According to the
[definition of LCA on Wikipedia](https://en.wikipedia.org/wiki/Lowest_common_ancestor):
"The lowest common ancestor is defined between two nodes p and q as the lowest
node T that has both p and q as descendants (where we allow **a node to be a
descendant of itself**)."

## Hints

1. One way to do this would be to record the path to `p` and the path to `q`. These
   two paths can be compared for the last node they have in common. This last node
   would be the LCA.

## Solutions

### Track paths

This solution follows the first hint, recording the paths to the target nodes
and then comparing these paths. This particular implementation combines the
`p` and `q` path determinations into a single traversal. This results in a
slight speed up but not a speed up from a big O perspective.

Given a tree with n nodes, the time complexity of this solution is O(n). That
is O(n) traversals are required to find the target nodes, and an up to O(n)
traversal is necessary to compare the returned lists. Thus the overall big O
is O(n). For the space complexity, the returned lists and the recursion used
to find the nodes could require a length / depth of n. Thus the space
complexity is also O(n).

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        List<TreeNode> ppath = new ArrayList<>();
        List<TreeNode> qpath = new ArrayList<>();
        lca(root, p, q, new ArrayList<>(), ppath, qpath);
        int i = 0;
        while (i < ppath.size() && i < qpath.size() && ppath.get(i) == qpath.get(i)) {
            i++;
        }
        return ppath.get(i - 1);
    }

    private void lca(TreeNode node, TreeNode p, TreeNode q,
            List<TreeNode> path, List<TreeNode> ppath, List<TreeNode> qpath) {
        if (node == null) {
            return;
        }
        path.add(node);
        // When we find a target node, record the path.
        if (node == p) {
            ppath.addAll(path);
        } else if (node == q) {
            qpath.addAll(path);
        }
        // If we have determined both paths, our traversal work is done.
        if (!ppath.isEmpty() && !qpath.isEmpty()) {
            return;
        }
        lca(node.left, p, q, path, ppath, qpath);
        lca(node.right, p, q, path, ppath, qpath);
        path.remove(path.size() - 1);
    }
}
```

### Smarter recursion

While the above solution works and has good running time, it is overly complex
for what needs to be done here. Since the nodes in the constraint of this
problem are guaranteed to exist in the tree, a fairly straight forward recursion
works. Here when `p` or `q` is seen, they are returned. If a recursive case is
entered that looked at both the left and right branches, and it returns nodes
from both invocations, then that is the lowest common ancestor. This approach
has O(n) time complexity but is trivially faster than the above solution per the
leeter timings.

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null) return null;
        if (root == p || root == q) return root;
        TreeNode lca1 = lowestCommonAncestor(root.left, p, q);
        TreeNode lca2 = lowestCommonAncestor(root.right, p, q);
        if (lca1 != null && lca2 != null) {
            return root;
        } else if (lca1 != null) {
            return lca1;
        } else {
            return lca2;
        }
    }
}
```
