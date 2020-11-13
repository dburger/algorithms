# Simplify Path

Given an **absolute path** for a file (Unix-style), simplify it. In other words,
convert it to the **canonical path**.

In a Unix-style file system, a period `'.'` refers to the current directory.
Furthermore, a double period `'..'` moves the directory up a level.

Note that the returned canonical path must always being with a slash `'/'`, and
there must be only a single slash `'/'` between two directory names. The last
directory name (if it exists) must not end with a trailing `'/'`. Also, the
canonical path must be the **shortest** string representing the absolute path.

## Hints

1. Split and process?

## Solutions

### Split and process

```java
class Solution {
    public String simplifyPath(String path) {
        String[] parts = path.split("/");
        Deque<String> d = new ArrayDeque<>();
        for (String part : parts) {
            if (part.length() == 0 || part.equals(".")) {
                continue;
            } else if (part.equals("..")) {
                if (!d.isEmpty()) {
                    d.removeLast();
                }
            } else {
                d.addLast(part);
            }
            // d.addLast(part);
        }
        StringBuilder buf = new StringBuilder();
        for (String part : d) {
            buf.append("/");
            buf.append(part);
        }
        return buf.length() == 0 ? "/" : buf.toString();
    }
}
```
