# Flood Fill

An `image` is represented by a 2-D array of integers, each integer representing
the pixel value of the image (from 0 to 65535).

Given a coordinate `(sr, sc)` representing the staring pixel (row and column) of
the flood fill, and a pixel value `newColor`, "flood fill" the image.

To perform a "flood fill", consider the starting pixel, plus any pixels
connected 4-directionally to thee starting pixel of the same color as the
starting pixel, plus any pixels connected 4-directionally to those pixels
(also with the same color as the starting pixel), and so on. Replace the color
of all of the aforementioned pixels with the `newColor`.

At the end, return the modified image.

## Hints

1. This is a commonly needed techique for working with various matrix problems.
   Recursion around the start pixel into its 4-directional neighbors seems the
   way to go.

## Solutions

### Expand recursively

This approach simply expands recursively. A couple of key points to note:

* It is easy to miss the early return if the target is already the correct
  color edge case.
* Note the checking if the point is out of bounds at the start of `expand`,
  instead of checking before each recursive call to expand, this keeps things
  much simpler.

```java
class Solution {
    public int[][] floodFill(int[][] image, int sr, int sc, int newColor) {
        int oldColor = image[sr][sc];
        if (oldColor == newColor) {
            return image;
        }
        expand(image, sr, sc, oldColor, newColor);
        return image;
    }

    private void expand(int[][] image, int r, int c, int oldColor, int newColor) {
        // First make sure (sr, sc) is in bounds.
        if (r < 0 || r >= image.length || c < 0 || c >= image[0].length) {
            return;
        }
        // Next check to see if qualifies as oldColor.
        if (image[r][c] != oldColor) {
            return;
        }
        image[r][c] = newColor;
        expand(image, r - 1, c, oldColor, newColor);
        expand(image, r + 1, c, oldColor, newColor);
        expand(image, r, c - 1, oldColor, newColor);
        expand(image, r, c + 1, oldColor, newColor);
    }
}
```
