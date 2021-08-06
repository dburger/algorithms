# Shortest Path in Binary Matrix

Given an `n x n` binary matrix `grid`, return the length of the shortest
**clear path** in the matrix. If there is no clear path, return `-1`.

A **clear path** in a binary matrix is a path from the **top-left** cell
(i.e.,  `(0, 0)`) to the **bottom-right** cell (i.e., `(n - 1, n - 1)` such
that:

*   All the visited cells of the path are `0`.
*   All the adjacent cells of the path are **8-directionally** connected
    (i.e., they are different and they share an edge or a corner).

The **length of a clear path** is the number of visited cells of this path.

## Hints

1. This kind of reeks of certain types of dynamic programming problems. Here,
   however, some breadth first traversal probably does the trick.

## Solutions

### Go breadth (not deep)

This solution uses a BFS style traversal to explore the reachable parts of the
grid in layers. The grid itself is used to mark progress. Once a cell has been
visited is is flipped to a `1` to prevent revisiting the same cell. This works
because obviously revisiting a cell would only result in a longer path through
that cell, thus the cells first visit is the best visit.

The time complexity of this algorithm is O(n * n), the dimensions of the grid,
as each cell may be visited in the traversal. The space complexity of this
solution is O(1). The input itself is used for marking visited cells.
Otherwise, only a constant amount of space is used regardless of the input.

```java
class Solution {
    // The eight directional offsets
    private int[][] offsets = {{-1, -1}, {-1, 0}, {-1, 1}, {0, -1}, {0, 1}, {1, -1}, {1, 0}, {1, 1}};

    public int shortestPathBinaryMatrix(int[][] grid) {
        int rows = grid.length;
        int cols = grid[0].length;
        // Queue to hold cells expressed in row major order.
        Deque<Integer> d = new LinkedList<>();

        if (grid[0][0] == 0) {
            // Mark as visited and add it to the queue.
            grid[0][0] = 1;
            d.add(0);
        }

        int path = 1;
        while (!d.isEmpty()) {
            // Process one level at a time and increment ath below.
            int size = d.size();
            while (size-- > 0) {
                int pos = d.removeFirst();
                int r = pos / cols;
                int c = pos % cols;
                if (r == rows - 1 && c == cols - 1) {
                    // We reach the bottom right.
                    return path;
                }
                for (int[] offset : offsets) {
                    int nr = r + offset[0];
                    int nc = c + offset[1];
                    // If it is in bounds and also traversable, add it to the quque.
                    if (nr > -1 && nr < rows && nc > -1 && nc < cols && grid[nr][nc] == 0) {
                        // Mark as visited and add it to the queue.
                        grid[nr][nc] = 1;
                        d.addLast(nr * cols + nc);
                    }
                }
            }
            path++;
        }

        return -1;
    }
}
```
