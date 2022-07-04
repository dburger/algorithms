# Course Schedule

There are a total `numCourses` courses you have to take, labeled from `0`
to `numCourses - 1`. You are given an array `prerequisites` where
`prerequisites[i] = [ai, bi]` indicates that you **must** take course `bi`
first if you want to take course `ai`.

- For example, the pair `[0, 1]`, indicates that to take course `0` you
  have to first take course `1`.

Return `true` if you can finish all courses. Otherwise, return `false`.

## Hints

1. Think of working from those already cleared with no dependencies.

## Solutions

### Queue em up, knock them down

This solution uses a `Queue` approach where courses with no dependencies are
enqueued. Then we proceed to remove items from the queue. If we consider each
removal to correspond with completing the course it is clear that we can
decrement the dependency count in each dependent course. If the dependent's
count goes to 0 we can then enqueue that course. We continue this until the
queue is empty.

```java
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        // Dependency count for each course.
        int[] dependencies = new int[numCourses];
        // Map from course to courses it clears a dependency for.
        Map<Integer, List<Integer>> preqs = new HashMap<>();
        for (int[] p : prerequisites) {
            int c1 = p[0];
            int c2 = p[1];
            // Must take c2 before you can take c1.
            dependencies[c1]++;
            List<Integer> l = preqs.get(c2);
            if (l == null) {
                l = new ArrayList<>();
                preqs.put(c2, l);
            }
            l.add(c1);
        }

        Queue<Integer> q = new LinkedList<>();
        for (int i = 0; i < numCourses; i++) {
            if (dependencies[i] == 0) {
                q.add(i);
            }
        }

        int count = 0;
        while (!q.isEmpty()) {
            int c = q.remove();
            // Course c is clear to take, count it and decrement for
            // its dependents.
            count++;
            for (int d : preqs.getOrDefault(c, Collections.emptyList())) {
                if (--dependencies[d] == 0) {
                    q.add(d);
                }
            }
        }

        return count == numCourses;
    }
}
```
