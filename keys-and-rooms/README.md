# Keys and Rooms

There are `n` rooms labeled from `0` to `n - 1` and all the rooms are locked
except for room `0`. Your goal is to visit all the rooms, however, you cannot
enter a locked room without having its key.

When you visit a room, you may find a set of **distinct keys** in it. Each key
has a number on it, denoting which room it unlocks, and you can take all of
them with you to unlock the other rooms.

Given an array `rooms` where `room[i]` is the set of keys that you can obtain
if you visited room `i`, return `true` if you can visit **all** the rooms, or
`false` otherwise.

## Hints

1. Think how you can keep track of rooms you can unlock and the room to try
   next.

## Solutions

This is kind of a clever little problem but perhaps too easy to be listed as a
medium. You can solve it by exercising a BFS (breadth first search) pattern.
The solution works by keeping a queue of rooms that are unlocked, and you
explore which keys that room gives you in each iteration. Those keys are added
to the key if you haven't explored them yet, and you continue until the queue
is empty. If you have visited all the rooms when the queue is empty, you can
in fact unlock all the rooms.

The time complexity of this solution is `O(n^2)` where `n` is the number of
rooms. This comes from potentially examining each key `n` times. This comes
from the fact that the problem solution doesn't state that each room can
essentially contain all the other keys. The space complexity is `O(n)`. This
comes from storing the queue of rooms to examine in order and the `visited`
set to track which rooms have already been unlocked / had their keys
extracted.

### Queue me

```java
class Solution {
    public boolean canVisitAllRooms(List<List<Integer>> rooms) {
        Queue<Integer> q = new LinkedList<>();
        Set<Integer> visited = new HashSet<>();
        q.add(0);
        visited.add(0);
        while (!q.isEmpty()) {
            Integer room = q.remove();
            for (Integer key : rooms.get(room)) {
                if (visited.add(key)) {
                    q.add(key);
                }
            }
        }
        return visited.size() == rooms.size();
    }
}
```
