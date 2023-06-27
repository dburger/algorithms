# Island Perimeter

You are given a `row x col` `grid` representing a map where `grid[i][j] = 1`
represents land and `grid[i][j] = 0` represents water.

Grid cells are connected **horizontally/vertically** (not diagonally). The
`grid` is completely surrounded by water, and there is exactly one island
(i.e., one or more connected land cells).

The island doesn't have "lakes", meaning the water inside isn't connected to
the water around the island. One cell is a sware with side length 1. The grid
is rectangular, width and height don't exceed 100. Determine the perimeter of
the island.

## Hints

1. The problem constraints give a lot away
1. Can you make it faster?

## Solutions

### Let er fly

Since the problem constraints say there is only one island the problem
merely reduces to counting the water exposed perimeter of every land cell.
This can be done with one iteration and a counting method. The code is rather
simple, with the irritation of checking if we are at a boundary where the
boundary is considered to be water.

The time complexity of this algorithm is O(m * n) where m is the number of rows
and n is the number of columns. The space complexity is O(1) as additional
space is needed other than some counting space that is independent of the
input size.

```java
class Solution {
    public int islandPerimeter(int[][] grid) {
        int count = 0;
        for (int col = 0; col < grid[0].length; col++) {
            for (int row = 0; row < grid.length; row++) {
                if (grid[row][col] == 1) {
                    count += count(grid, row, col);
                }
            }
        }
        return count;
    }

    public int count(int[][] grid, int row, int col) {
        int count = 0;
        if (col == 0 || grid[row][col - 1] == 0) {
            count++;
        }
        if (col == grid[0].length - 1 || grid[row][col + 1] == 0) {
            count++;
        }
        if (row == 0 || grid[row - 1][col] == 0) {
            count++;
        }
        if (row == grid.length - 1 || grid[row + 1][col] == 0) {
            count++;
        }
        return count;
    }
}
```

### Cound just the island

We might be able to tweak a little better performance out of the above if we
only count the permimeter of the single island and try to ignore cells that
aren't island. Of special note in this problem is the usage of `2` to prevent
infinite recursion by marking cells that have already been explored.

We end up with the same O(m * n) time complexity of the above. In practice,
however, this algorithm has the possibility of finishing before it examines
every cell. Thus depending on the input it may run faster than the prior
approach. The space complexity takes a hit, however. Because this approach
uses recursion it is possible for quite a few frames to build up on the stack.
The space complexity is O(m * n).

```java
class Solution {
    public int islandPerimeter(int[][] grid) {
        int count = 0;
        for (int col = 0; col < grid[0].length; col++) {
            for (int row = 0; row < grid.length; row++) {
                if (grid[row][col] == 1) {
                    return count(grid, row, col);
                }
            }
        }
        return 0;
    }

    public int count(int[][] grid, int row, int col) {
        if (row < 0 || row > grid.length - 1 || col < 0 || col > grid[0].length - 1) {
            return 0;
        }
        if (grid[row][col] != 1) {
            return 0;
        }
        int count = 0;
        if (col == 0 || grid[row][col - 1] == 0) {
            count++;
        }
        if (col == grid[0].length - 1 || grid[row][col + 1] == 0) {
            count++;
        }
        if (row == 0 || grid[row - 1][col] == 0) {
            count++;
        }
        if (row == grid.length - 1 || grid[row + 1][col] == 0) {
            count++;
        }
        grid[row][col] = 2;
        return count +
            count(grid, row - 1, col) +
            count(grid, row + 1, col) +
            count(grid, row, col - 1) +
            count(grid, row, col + 1);
    }
}
```
