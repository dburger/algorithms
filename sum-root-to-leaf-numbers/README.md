# Sum Root to Leaf Numbers

Given a binary tree containing digits from `0-9` only, each root-to-leaf path
could represent a number.

An example is a the root-to-leaf path `1->2->3` which represents the number
`123`.

Find the total sum of all root-to-leaf numbers.

## Hints

You need to iterate the tree and accumulate when you hit a leaf node.

## Solutions

### Accumulate to a list

This solution accumlates the path to a leaf node in a list. When a leaf node
is encountered, the accumulated number can be added. Note the usage of
[horner's rule](https://en.wikipedia.org/wiki/Horner%27s_method)
to bulid the number.

```java
class Solution {
    public int sumNumbers(TreeNode root) {
        if (root == null) {
            return 0;
        }
        List<Integer> path = new ArrayList<>();
        List<Integer> paths = new ArrayList<>();
        s(root, path, paths);
        int totalSum = 0;
        for (int i : paths) {
            totalSum += i;
        }
        return totalSum;
    }
    private void s(TreeNode node, List<Integer> path, List<Integer> paths) {
        path.add(node.val);
        if (node.left == null && node.right == null) {
            int sum = 0;
            for (int i : path) {
                sum = 10 * sum + i;
            }
            paths.add(sum);
            path.remove(path.size() - 1);
            return;
        }
        if (node.left != null) {
            s(node.left, path, paths);
        }
        if (node.right != null) {
            s(node.right, path, paths);
        }
        path.remove(path.size() - 1);
    }
}
```

### Horner's all the way down

Here we get a lot more clever. Instead of storing the actual paths,
we just use horner's rule as we travel down, and undo it as we
recurse back up. The current sum and total sum are tracked in an
accumulator object during the recurision.

```java
class Solution {
    private class Sum {
        int current;
        int total;
    }

    public int sumNumbers(TreeNode root) {
        Sum sum = new Sum();
        sn(root, sum);
        return sum.total;
    }

    private void sn(TreeNode node, Sum sum) {
        if (node == null) {
            return;
        }
        sum.current = 10 * sum.current + node.val;
        if (node.left == null && node.right == null) {
            sum.total += sum.current;
        }
        sn(node.left, sum);
        sn(node.right, sum);
        sum.current -= node.val;
        sum.current /= 10;
    }
}
```
