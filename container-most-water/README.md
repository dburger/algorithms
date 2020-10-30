# Container With Most Water

Given `n` non-negative integers `a1, a2, ..., an`, where each represents a point
at coordinate `(i, ai)`, `n` vertical lines are drawn such that the two
endpoints of the line `i` is at `(i, ai)` and `(i, 0)`. Find two lines, which,
together with the x-axis forms a container, such that the container contains the
most water.

**Notice** that you may not slant the container.

## Hints

1. Start at the extremes and work inwards. Should you move in from the side with
   the shorter line or the taller line?

## Solutions

### The squeeze

```java
class Solution {
    private int area(int l, int r, int[] height) {
        return (r - l) * Math.min(height[l], height[r]);
    }

    public int maxArea(int[] height) {
        int l = 0;
        int r = height.length - 1;
        int max = area(l, r, height);
        while (l < r) {
            if (height[l] < height[r]) {
                l++;
            } else {
                r--;
            }
           max = Math.max(max, area(l, r, height));
        }
        return max;
    }
}
```
