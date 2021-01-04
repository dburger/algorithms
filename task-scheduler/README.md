# Task Scheduler

Given a characters array `tasks`, representing the tasks a CPU needs to do,
where each letter represents a different task. Tasks could be done in any
order. Each task is done in one unit of time. For each unit of time, the CPU
could complete either one task or just be idle.

However, there is a non-negative integer `n` that represents the cooldown
period between two **same tasks** (the same letter in the array), that is
there must be at least `n` units of time between any two same tasks.

Return *the least number of units of times that the CPU will take to finish
all the given tasks*.

## Hints

1. Since all tasks take the same single time unit and all have the same cooldown
   there is a rather obvious dominance of the task with the highest count. A
   solution can be made around the dominance of this task.

## Solutions

### Groups of tasks

This solution follows the hint above. The key to the solution is that for the
task with the max count, say `T` with count `x`, there needs to be `x - 1`
groups with the necessary cooldown spots between those `T`s. The last `T` would
not have another `T` after it so no forced spots are necessary there. Since all
of the other types of tasks have counts that are less than or equal to `T`'s
count they can simply be feathered into the cooldown spots. Thus the number of
cooldown spots is `(x - 1) * n`. In the loop we reduce the cooldown spots my the
minimum of the number of groups or the count for the particular task. The reason
the min is required is because the count of the particular task could be the
same as `T`'s, and in that case we let the one extra task go after the last `T`,
not taking a cooldown spot.

At the end we either have cooldown spots left over or the number has gone to zero
or less. If the cooldown spots is still over zero, the number of time units
required is `tasks.length` plus the cooldown spots that couldn't be filled. If
cooldown spots is <= 0 then all cooldown spots are filled and the number of time
units required is `tasks.length`.

```java
class Solution {
    public int leastInterval(char[] tasks, int n) {
        if (n == 0) return tasks.length;
        if (tasks.length == 0) return 0;
        int[] counts = new int[26];
        for (char c : tasks) {
          counts[c - 'A']++;
        }
        Arrays.sort(counts);
        int chunks = counts[25] - 1;
        // Minimum required cooldown spots.
        int idleSpots = chunks * n;
        for (int i = 24; i >= 0; i--) {
          int count = counts[i];
          if (count == 0) break;
          idleSpots -= Math.min(chunks, count);
        }
        return idleSpots > 0 ? tasks.length + idleSpots : tasks.length;
    }
}
```
