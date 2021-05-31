# Permutations

Given an array of `nums` distinct integers, return *all the possible
permutations*. You can return the answer in **any order**.

## Hints

1. This is much like
   [Generate String Permutations](../generate-string-permutations). The same
   techniques apply. Think in terms of "consumed" and "remaining" buckets
   with an accumulator.
1. This problem is typically done as a "backtracking" problem. That is, you
   advance towards a solution by adding an element and then recurse. When you
   back out of that solution you remove that element and then advance with
   a different element added.

## Solutions

Below I laboriously go through the different approaches to handling this
problem. This includes starting with initial approaches that are not as
efficent. Primarily this has to do with starting approaches that manipulate
data structures in more expensive ways such as creating new copies of arrays
and lists and shuffling elements. The final two solutions shown here are done
in more optimal ways.

### Consumed and remaining, handling as `int[]`

Here we follow the "consumed" and "remaining" pattern with a recursive
approach that inserts into an accumulator and handles the data structures as
`int[]`. This involves a lot of copying from one array to another, however,
the performance is still good against the leeter grader as this solution gets
a 0 ms completion time.

The time complexity for this solution is O(n!), as each permutation must be
produced.

```java
class Solution {
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> acc = new ArrayList<>();
        permute(nums, new int[] {}, acc);
        return acc;
    }

    private static void permute(int[] source, int[] consumed, List<List<Integer>> acc) {
        if (source.length == 0) {
            List<Integer> l = new ArrayList<>();
            for (int i = 0; i < consumed.length; i++) {
                l.add(consumed[i]);
            }
            acc.add(l);
	    return;
        }
        for (int i = 0; i < source.length; i++) {
            int[] s = new int[source.length - 1];
            int pos = 0;
            for (int j = 0; j < source.length; j++) {
                if (j != i) {
                    s[pos++] = source[j];
                }
            }
            int[] c = new int[consumed.length + 1];
            System.arraycopy(consumed, 0, c, 0, consumed.length);
            c[c.length - 1] = source[i];
            permute(s, c, acc);
        }
    }
}
```

### Consumed and remaining, handling as `List<Integer>`

Here we follow the same "consumed" and "remaining" pattern with a recursive
approach that inserts into an accumulator and handles the data structures as
`List<Integer>`. Using lists we avoid the creation of many `int[]` instances
of the prior solution, and instead manipulate the same `source` and `consumed`
lists throughout. This doesn't really speed things up, however, as the
removals and inserts into the `ArrayList` involve shuffling of positions in
the array.

The time complexity for this solution is O(n!), as each permutation must be
produced.

```java
class Solution {
    public List<List<Integer>> permute(int[] nums) {
        List<Integer> source = Arrays.stream(nums)
                .boxed()
                .collect(Collectors.toCollection(ArrayList::new));
        List<List<Integer>> acc = new ArrayList<>();
        permute(source, new ArrayList<>(), acc);
        return acc;
    }

    private static void permute(List<Integer> source, ArrayList<Integer> consumed, List<List<Integer>> acc) {
        if (source.size() == 0) {
            acc.add(new ArrayList<>(consumed));
	    return;
        }
        for (int i = 0; i < source.size(); i++) {
            int val = source.remove(i);
            consumed.add(val);
            permute(source, consumed, acc);
            source.add(i, val);
            consumed.remove(consumed.size() - 1);
        }
    }
}
```

### Consumed and remaining, handling with `int[]` and `boolean[]`

Once again we follow the "consumed" and "remaining" pattern but we mark
consumed by sending along a boolean array that indicates which source
positions have been consumed. Presumably this should speed things up in
practice as the source copying is eliminated. This one clocks in at a 0 ms
leeter completion time.

The time complexity remains O(n!), as each permutation must be produced.

```java
class Solution {
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> acc = new ArrayList<>();
        permute(nums, new boolean[nums.length], new int[] {}, acc);
        return acc;
    }

    private static void permute(int[] source, boolean[] used, int[] consumed, List<List<Integer>> acc) {
        if (consumed.length == source.length) {
            List<Integer> l = new ArrayList<>();
            for (int i = 0; i < consumed.length; i++) {
                l.add(consumed[i]);
            }
            acc.add(l);
            return;
        }
        for (int i = 0; i < source.length; i++) {
            if (used[i]) {
                continue;
            }
            int[] c = new int[consumed.length + 1];
            System.arraycopy(consumed, 0, c, 0, consumed.length);
            c[c.length - 1] = source[i];
            used[i] = true;
            permute(source, used, c, acc);
            used[i] = false;
        }
    }
}
```

### Consumed and remaining, handling with `List<Integer>` and `boolean[]`

Now we combine what we have done above into the theoretically fastest
combination of approaches used above. The source is left unchanged as an
`int[]` and we mark consumed positions with a `boolean[]`. The consumed
values are communicated as a `List<Integer>` as adding and removing only from
the end of the list is not expensive.

While slightly faster in practice, the big O running time remains O(n!). We
still need to produce each permuation.

Jokes on us. While still O(n!) I thought this would be faster than the prior
solution in practice. Reason being, we can avoid the creation of all those
"consumed" `int[]` but using a list that we add and remove from. In practice,
it appears the creation of the `Integer` that go into that list is more
expensive than the creation and copy of the `int[]`. This solution typically
takes 1 ms under the leeter grader while the prior could do 0 ms.

```java
```

### Consumed and remaining, handling with `int[]` and `boolean[]`, reduced
    `int[]` creation

Talk about beating a dead horse. Now we take the solution above that uses an
`int[]` and `boolean[]` and avoid `int[]` creation by using a single array and
passing a length. By reusing a single `consumed` array and passing a length,
most of the instance creation and element shuffling of the prior solutions is
eliminated.

This version is also O(n!) as it produces each permutation. In practice it
should be the fastest of all the solutions shown here.

```java
class Solution {
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> acc = new ArrayList<>();
        int len = nums.length;
        permute(nums, new boolean[len], new int[len], 0, acc);
        return acc;
    }

    private static void permute(int[] source, boolean[] used, int[] consumed, int consumedLen, List<List<Integer>> acc) {
        if (consumedLen == source.length) {
            List<Integer> l = new ArrayList<>();
            for (int i = 0; i < consumedLen; i++) {
                l.add(consumed[i]);
            }
            acc.add(l);
            return;
        }
        for (int i = 0; i < source.length; i++) {
            if (used[i]) {
                continue;
            }
            consumed[consumedLen++] = source[i];
            used[i] = true;
            permute(source, used, consumed, consumedLen, acc);
            consumedLen--;
            used[i] = false;
        }
    }
}
```
