# Arrays and Strings

## 1.1 **Is Unique:**

Implement an algorithm to determine if a string has all unique characters.
What if you cannot use additional data structures?

### Solution 1

This obvious solution uses a `Set` to track if a character has been seen
before.

The time complexity of this solution is `O(n)` where `n` is the length of
the string `s`. The space complexity is also `O(n)` as we need to hold the
input characters in the `Set`.

```java
boolean allUnique(String s) {
    Set<Character> s = new HashSet<>();
    for (char c : s.toCharArray()) {
        if (!s.add(c)) {
            return false;
        }
    }
    return true;
}
```

### Solution 2

If we can assume a restricted character set we can dispense with the `Set`
and just use an array of boolean values. The time complexity remains the
same as above but the space complexity is reduced to `O(1)`.

```java
private static boolean allUnique(String s) {
    boolean[] seen = new boolean[256];
    for (int c : s.toCharArray()) {
        if (seen[c]) {
            return false;
        }
    }
    return true;
}
```

### Solution 3

Looking at the prior solution, assuming a limited character set, we can
return immediately if the number of characters in the input string is
bigger than the character set. This takes the time complexity down to
`O(1)` as the loop will never execute more than the character set size
number of times.

```java
private static boolean allUnique(String s) {
    if (s.length() > 256) {
        return false;
    }
    boolean[] seen = new boolean[256];
    for (int c : s.toCharArray()) {
        if (seen[c]) {
            return false;
        }
    }
    return true;
}
```

### What if you can't use additional data structures?

This extra constraint given in the problem can be accomplished in a couple
of ways. First, you could just do brute force and do a comparison of every
character to every other characters. This would result in an alogorithm that
didn't use additional data structures but run in `O(n^2)` time. Another
possible approach would be to sort the characters and then check for a
repeat of a neighbor. This would require `O(n * log(n))` for the sort.

## 1.2 **Check Permutation:**

Given two strings, write a method to decide if on is a permutation of the
other.

### Solution 1

An easy way to solve this problem is to sort the two input strings and compare
the result. If they are permutations of one another the sorted strings will
be equal.

The time complexity of this solution is `O(a * log(a) + b * log(b))` where
`a` is the length of the first string and `b` is the length of the second.

```java
boolean isPerm(String a, String b) {
    if (a.length() != b.length()) {
        return false;
    }
    char[] ca = a.toCharArray();
    char[] cb = b.toCharArray();
    Arrays.sort(ca);
    Arrays.sort(cb);
    return Arrays.equals(ca, cb);
}
```

### Solution 2

This more efficient solution works by creating a counting map for one
string and then subtracting back out of the counting map with the other.
If a value is not present, or if it reduces the count to a negative value,
false is returned. Otherwise, true is returned.

The time complexity of this solution is `O(n + m)` where `n` and `m` are
the lengths of the two different strings. The space complexity is `O(m)`
where `m` is the length of string `b`, as the counts for this string
are kept in the counting map.

Note that if we assume a restricted character set we could use a boolean
array instead of the `Map`. This reduces the space complexity to `O(1)`.

```java
private static boolean isPerm(String a, String b) {
    Map<Character, Integer> counts = new HashMap<>();
    for (char c : b.toCharArray()) {
        counts.put(c, counts.getOrDefault(c, 0) + 1);
    }
    for (char c : a.toCharArray()) {
        Integer old = counts.get(c);
        if (old == null || old == 0) {
            return false;
        }
        counts.put(c, old - 1);
    }
    return true;
}
```

## 1.3 **URLify:**

Write a method to replace all spaces in a string with `%20`. You may assume
that the string has sufficient space at the end to hold the additional
characters, and that you are given the "true" length of the string. (Note:
If implementing in Java, please use a character array so that you can
perform this operation in place.)

### Solution 1

This solution works by first counting the spaces. This benefits us by figuring
out where the end of the resultant string is. Thus we can start in reverse and
avoid overwriting the string. That is we are writing into the extra spaces that
are indicated in the problem statement. The second pass copies the values back
into the character array. When the source character is a space, we copy the
three values of `%20` and decrement destination accordingly.

The time complexity of this solution is `O(n)` where `n` is the length of
the `input` character array. This comes from making two passes through the
input. The space complexity is `O(1)` as the space used to solve the problem
is not dependent on the input size.

```java
void urlify(char[] input, int len) {
    int numSpace = 0;
    for (int i = 0; i < len; i++) {
        if (input[i] == ' ') {
            numSpace++;
        }
    }
    int dest = len + 2 * numSpace;
    len--;
    for (; len > -1; len--) {
        char c = input[len];
        if (c == ' ') {
            input[dest] = '0';
            input[dest - 1] = '2';
            input[dest - 2] = '%';
            dest -= 3;
        } else {
            input[dest] = c;
            dest--;
        }
    }
}
```

## 1.4 **Palindrome Permutation:**

Given a string, write a function to check if it is a permutation of a
palindrome. A palindrome is a word or phrase that is the same forwards
or backwards. A permutation is a rearrangement of letters. The palindrome
does not need to be limited to just dictionary words.

### Solution 1

Think of the character counts in a palindrome. Even character counts can be
split to the left and right of center. There can be 0 or 1 odd character
counts for a middle character. This solution exploits this fact by first
noting the counts, and then seeing how many odd counts there are.

Note that this solution assumes a limited character set and thus uses a
boolean array. Alternatively a counting `Map` could be used with only slightly
more complicated code.

The time complexity of this solution is `O(n)` where `n` is the size of the
input string. The space complexity for this limited character set approach
is `O(1)`. For the `Map` approach, it would be `O(n)`.

```java
boolean isPalindromePerm(String input) {
    int[] counts = new int[128];
    for (int c : input.toCharArray()) {
        counts[c]++;
    }

    int odds = 0;
    for (int i = 0; i < counts.length; i++) {
        if (counts[i] % 2 == 1) {
            odds++;
        }
    }
    return odds == 0 || odds == 1;
}
```

## 1.5 **One Away:**

There are three types of edits that can be performed on strings:
insert a character, remove a character, or replace a character.
Given two strings, write a function to check if they are one edit
(or zero edits) away.

### Solution 1

There are three different types of edits that are valid. A key simplification
is that one of them can be eliminated. Remove a character from the longer
is equivalent to insert a character for the shorter. Thus we can get by
by breaking this into two cases. Here we do the `oneReplace` and `oneInsert`
cases. If the strings are of equal lengths we attempt `oneReplace`. This
is merely done by counting diffs and the positive case being where we find
one diff. For the `oneInsert` case, we iterate on the shorter until we find
the differing character. At this point we begin a second iteration where we
skip a character in the longer string and try to get to the end of both strings
with the rest all being equal. If we do, then one insert before the differing
position in the shorter string would make them equal.

The time complexity of this solution is `O(n)` where `n` is the length of
the longer string. The space complexit is `O(1)`.

```java
boolean oneEditAway(String s1, String s2) {
    if (s1.length() == s2.length()) {
        return s1.equals(s2) || oneReplace(s1, s2);
    } else if (s1.length() == s2.length() - 1) {
        return oneInsert(s1, s2);
    } else if (s2.length() == s1.length() - 1) {
        return oneInsert(s2, s1);
    } else {
        return false;
    }
}

boolean oneReplace(String s1, String s2) {
    int diffs = 0;
    for (int i = 0; i < s1.length(); i++) {
        if (s1.charAt(i) != s2.charAt(i)) {
            diffs++;
            if (diffs == 2) {
                return false;
            }
        }
    }
    return true;
}

boolean oneInsert(String shorter, String longer) {
    int i = 0;
    while (i < shorter.length() && shorter.charAt(i) == longer.charAt(i)) {
        i++;
    }
    while (i + 1 < longer.length() && shorter.charAt(i) == longer.charAt(i + 1)) {
        i++;
    }
    return i == shorter.length();
}
```

## 1.6 **String Compression:**

Implement a method to perform basic string compression using the counts
of repeated characters. For example, the string `aabcccccaaa` would
become `a2b1c5a3`. If the "compressed" string would not become smaller
than the original string, your method should return the original string.
You can assume the string has only uppercase and lowercase letters (a-z).

### Solution 1

Here we can proceed through the string keeping track of the `last` character
and the `count` of how many of those we have seen in a row. When `c` is
not equal to `last`, we add its translation to the string builder. We keep
doing this until we either finish the string or determine that we already
have a string builder larger than the orginal string.

The time complexity of this algorithm is `O(n)` where `n` is the length of
the input string. This comes from the need to iterate through the input.
The space complexity is also `O(n)`. The `StringBuilder` may need to hold
up to `n` characters.

```java
void append(StringBuilder buf, char c, int count) {
    buf.append(c);
    if (count > 1) {
        buf.append(count);
    }
}

String tryCompress(String s) {
    if (s.length() == 0 || s.length() == 1) {
        return s;
    }
    StringBuilder buf = new StringBuilder();
    char last = s.charAt(0);
    int count = 1;
    for (int i = 1; i < s.length() && buf.length() < s.length(); i++) {
        char c = s.charAt(i);
        if (c == last) {
            count++;
        } else {
            append(buf, last, count);
            count = 1;
            last = c;
        }
    }
    append(buf, last, count);
    return buf.length() < s.length() ? buf.toString() : s;
}
```

## 1.7 **Rotate Matrix:**

Given an image represented by an `NxN` matrix, where each pixel in the
image is 4 bytes, write a method to rotate the image by 90 degrees. Can
you do this in place?

### Solution 1

This is tricky and because I haven't ran this code I almost certainly made
an indexing error below. This works by storing the "bottom left" in a temporary
variable and then going around the matrix copying the rotate value into its
new slot. The `temp` variable takes care of the final copy for each round.

How do you figure out the indexing? Well, `j` moves faster than `i`. Think of
which row or column index is moving faster for each position of the matrix.
That is, top left, top right, bottom right, and bottom left. For the top left,
we are moving from right to left before we move down a row. Thus the faster
`j` would be used in the column index and the slower `i` would be used in the
row index.

The time complexity of this solution is `O(n)` where `n` is the dimension of
the input square matrix. The space complexity is `O(1)`.

```java
void rotate(int[][] matrix) {
    int size = matrix.length;
    for (int i = 0; i < size; i++) {
        for (int j = 0; j < size; j++) {
            // Record the bottom left corner.
            int temp = matrix[size - 1 - j][i];
            // Bottom left = bottom right.
            matrix[size - 1 - j][i] = matrix[size - 1 - i][size - 1 - j];
            // Bottom right = top right.
            matrix[size - 1 - i][size - 1 - j] = matrix [j][size - 1 - i];
            // Top right = top left.
            matrix [j][size - 1 - i] = matrix[i][j];
            // Top right = recorded bottom left.
            matrix[i][j] = temp;
        }
    }
}
```

## 1.8 **Zero Matrix:**

Write an algorithm such that if an element in an `NxN` matrix is 0, its
entire row and column are set to 0.

### Solution 1

Here we solve the problem with a two pass approach. In the first pass we flag
a boolean array if a row or column should be "zeroed out." In the second pass
we do the actual zeroing.

The time complexity of this solution is `O(n)` where `n` is the dimension of
the matrix. The space complexity is also `O(n)` to hold the flagging arrays.

```java
void zeroOut(int[][] input) {
    boolean[] rows = new boolean[input.length];
    boolean[] cols = new boolean[input[0].length];
    for (int row = 0; row < input.length; row++) {
        for (int col = 0; col < input.length; col++) {
            if (input[row][col] == 0) {
                rows[row] = true;
                cols[col] = true;
            }
        }
    }

    for (int row = 0; row < rows.length; row++) {
        if (rows[row]) {
            for (int col = 0; col < cols.length; col++) {
                input[row][col] = 0;
            }
        }
    }

    for (int col = 0; col < cols.length; col++) {
        if (cols[col]) {
            for (int row = 0; row < rows.length; row++) {
                input[row][col] = 0;
            }
        }
    }
}
```

## 1.9 **String Rotation:**

Assume you have a method `isSubstring` which checks if one word is a
substring of another. Given two strings `s1` and `s2`, write code to
check if `s2` is a rotation of `s1` using only one call to `isSubstring`.

### Solution 1

I consider this problem one of those "spot the trick" problems. The trick
is concatenating the possibly rotated string `s2` to itself. If that string is a
rotation of the other string `s1` then `s1` will be found in this concatenation.

The solution is `O(n)` time complexity where `n` is the length of `s2`. The
space complexity is `O(n)`, as we have to make a string twice as long to do
the concatenation.

```java
boolean isRotation(String s1, String s2) {
    if (s1.length() != s2.length()) {
        return false;
    }
    String haystack = s2 + s2;
    return haystack.contains(s1);
}
```

## Linked Lists

## 2.1 **Remove Dups:**

Write code to remove duplicates from an unsorted linked list.
How would you solve this problem if a temporary buffer is not
allowed?

### Solution 1

This solution works by keeping `prev` and `curr` pointers. We advance
`curr` through the linked list and set it as the `next` pointer in
`prev` when it represents a new value for the list. We recognize new
values with the usage of a `Set`. If it does not represent a new value,
we skip it.

The time complexity of this algorithm is `O(n)` where `n` is the size
of the linked list. The space complexity is `O(1)`.

```java
void removeDups(Node head) {
    if (head == null) {
        return;
    }
    Set<Integer> s = new HashSet<>();
    s.add(head.val);
    Node prev = head;
    Node curr = head.next;
    while (curr != null) {
        if (s.add(curr.val)) {
            prev.next = curr;
            prev = curr;
            curr = curr.next;
        } else {
            curr = curr.next;
        }
    }
    prev.next = null;
}
```

### Solution 2

To solve for the temporary buffer is not allowed case (our `Set` is not
allowed), we can solve the problem by using two pointers. In this approach
we use a `curr` pointers and `runner` pointers. We advance `runner` ahead
of `curr` and use it to skip duplicates of the value in `curr`.

While this reduces the space complexity to `O(1)`, the time complexity becomes
`O(n^2)`.

```java
void removeDups(Node head) {
    if (head == null) {
        return;
    }
    Node curr = head;
    while (curr != null) {
        Node runner = curr;
        while (runner.next != null) {
            if (runner.next.val == curr.val) {
                runner.next = runner.next.next;
            } else {
                runner = runner.next;
            }
        }
        curr = curr.next;
    }
}
```

## 2.2 **Return Kth to Last:**

Implement an algorithm to find the kth to last element of a singly
linked list.

### Solution 1

No need to get fancy here. The simplest algorithm has an optimum `O(n)`
time complexity and `O(1)` space complexity where `n` is the length of the
list. We just use two passes. The first pass determines the count. The
second pass retrieves the correct element.

```java
Node kth(Node head, int k) {
    int count = 0;
    Node curr = head;
    while (curr != null) {
        count++;
        curr = curr.next;
    }

    int target = count - k;
    curr = head;
    while (target > 0) {
        curr = curr.next;
        target--;
    }
    return curr;
}
```

### Solution 2

Can we do this with recursion? Why yes, yes we can. Here we implement a
recursive approach by returning a `NodePosition` object that contains a
node and its position from the end. At the end of the list we include a
`null` node and `0` in this object. We replace the `node` and increment
`position` until `position == k`. At that point we merely pass the correct
node back up the call stack.

The time complexity of this solution is `O(n)` where `n` is the length of
the list. Likewise, the space complexity is also `O(n)`. This comes from
the stack frames that recurse out to the end of the list.

```java
class NodePosition {
    Node node;
    int position;

    NodePosition(Node node, int position) {
        this.node = node;
        this.position = position;
    }
}

Node kth(Node head, int k) {
    NodePosition np = kthNode(head, k);
    return np.node;
}

NodePosition kthNode(Node node, int k) {
    if (node == null) {
        return new NodePosition(null, 0);
    } else {
        NodePosition np = kthNode(node.next, k);
        if (np.position < k) {
             np.position++;
             np.node = node;
        }
        return np;
    }
}
```

### Solution 3

A final obvious solution avoids the double pass of the first solution by
walking one pointer out `k` hops in front of another, and then advancing
both until the first pointer hits the end of the list. At that point the
second pointer is on the kth node.

This solution has time complexity `O(n)` and space complexity `O(1)`.


```java
Node kth(Node head, int k) {
    Node n1 = head;
    Node n2 = head;
    while (k > 0) {
        n1 = n1.next;
        k--;
    }
    while (n1 != null) {
        n1 = n1.next;
        n2 = n2.next;
    }
    return n2;
}
```

## 2.3 **Delete Middle Node:**

Implement an algoirthm to delete a node in the middle (i.e., any node but
the first and last node, no necessarily the exact middle) of a singly
linked list, given only access to that node.

### Solution 1

This can't be done, right? Well no it can't. Not if you literally mean to
delete the node. But what if you allow one to copy values. Then yes, yes it
can be done.

Here we copy the value of the next node into the deleted node and then skip
that next node.

The time and space complexity of this solution is `O(1)`.

```java
void deleteNode(Node node) {
    if (node == null || node.next == null) {
        throw new IllegalArgumentException("Invalid node passed to deleteNode");
    }
    node.val = node.next.val;
    node.next = node.next.next;
}
```

## 2.4 **Partition:**

Write code to partition a linked list around a value x, such that all nodes
less than x come before all nodes greater than or equal to x. If x is
contained within the list, the values of x only need to be after elements
less than x. The partition element x can appear anywhere in the "right
partition"; it does not need to appear between the left and right
partitions.

### Solution 1

Not much complexity here but a lot of bookeeping. We keep pointers for the
head and current of the left and right partitions and then piece them together
at the end. Note that the possibility of empty lists must be handled.

The time complexity of this solution is `O(n)` where `n` is the length of the
input list. The space complexity is `O(1)`.

```java
Node partitionList(Node head, int val) {
    Node leftHead = null;
    Node leftCurr = null;
    Node rightHead = null;
    Node rightCurr = null;

    while (head != null) {
        if (head.val < val) {
            if (leftHead == null) {
                leftHead = leftCurr = head;
            } else {
                leftCurr.next = head;
                leftCurr = head;
            }
        } else {
            if (rightHead == null) {
                rightHead = rightCurr = head;
            } else {
                rightCurr.next = head;
                rightCurr = head;
            }
        }
        head = head.next;
    }

    if (leftHead == null) {
        leftHead = rightHead;
    } else if (rightHead == null) {
        leftCurr.next = null;
    } else {
        leftCurr.next = rightHead;
        rightCurr.next = null;
    }
    return leftHead;
}
```

## 2.5 **Sum Lists:**

You have two numbers represented by a linked list, where each node contains
a single digit. The digits are stored in reverse order, such that the 1's
digit is at the head of the list. Write a function that adds the two
numbers and returns the sum as a linked list. Suppose the digits are stored
in forward order. Repeat the above problem.

### Solution 1

Solving this problem can be done by iterating through the lists and adding the
correct value to a `sum` variable. What is the correct value? Well the first
value would be a "one's" digit, the second a "ten's" digit, and so on. The
returned list can be reconstructed with the typical mod 10 and divide by ten
peeling off of each digit.

The time complexity of this solution is `O(n)` where `n` is the length of the
longer list. The space complexity is `O(1)`.

```java
Node sumLists(Node n1, Node n2) {
    int sum = 0;
    int i = 0;
    while (n1 != null || n2 != null) {
        int mult = (int) Math.pow(10, i++);
        if (n1 != null) {
            sum += mult * n1.val;
        }
        if (n2 != null) {
            sum += mult * n2.val;
        }
        n1 = n1.next;
        n2 = n2.next;
    }
    Node head = new Node(null, sum % 10);
    Node curr = head;
    sum /= 10;
    while (sum > 0) {
        curr.next = new Node(null, sum % 10);
        sum /= 10;
        curr = curr.next;
    }
    return head;
}
```

### Solution 2 (forward order)

Doing this in forward order brings a couple of wrinkles. First, while we can
use [horner's rule](https://en.wikipedia.org/wiki/Horner%27s_method) to sum
the numbers, we must sum them independently because the lists may have different
lengths. Second, presumably the returned list must be with most significant digit
first. This means we can't use the mod 10 approach to get the digits. Instead,
we divide by the highest power of 10 used and then mod by that value. Then we
proceed to the next smaller power of 10.

The time complexity and space complexity of this solution remains the same as
the prior, at `O(n)` and `O(1)` respectively.

```java
Node sumLists(Node n1, Node n2) {
    int n1Sum = 0;
    int n2Sum = 0;
    int pows = 0;
    while (n1 != null || n2 != null) {
        if (n1 != null) {
            n1Sum = 10 * n1Sum + n1.val;
        }
        if (n2 != null) {
            n2Sum = 10 * n2Sum + n2.val;
        }
        n1 = n1.next;
        n2 = n2.next;
        pows++;
    }
    int sum = n1Sum + n2Sum;
    int divider = (int) Math.pow(10, pows--);
    Node head = new Node(null, sum / divider);
    sum %= divider;
    Node curr = head;
    while (pows > -1) {
        divider = (int) Math.pow(10, pows--);
        curr.next = new Node(null, sum / divider);
        curr = curr.next;
        sum %= divider;
    }
    return head;
}
```

## 2.6 **Palindrome:**

Implement a function to check if a linked list is a palindrome.

### Solution 1

Obviously if you materialize the linked list into a string, this problem becomes
a trivial check if a string is a palindrome. We'll assume that isn't sufficient
for this problem. Thus, we implement a recursive solution.

In this solution we pass a `curr` node and a `NodeWrapper` down a recursive
chain. The `NodeWrapper` is used to hold `head` and then to advance head as we
back up the call chain. We first call all the way down to the end of the list,
then as we back out, we check if the `curr.val` is equal to `wrapper.curr.val`.
If it is we advance `wrapper.curr` and we return true. If it is not we return
false. A true return causes a similar execution to occur up the chain. If we
get all the way back to the first invocation with no `false`, we have a
palindrome.

Let me emphasize how neat this solution is. We use a wrapped `Node` to advance
forward through the list while the recursion backs up through the list.

```java
boolean palindrome(Node head) {
    return palindrome(head, new NodeWrapper(head));
}

boolean palindrome(Node curr, NodeWrapper wrapper) {
    if (curr == null) {
        return true;
    }
    boolean valid = palindrome(curr.next, wrapper);
    if (!valid) {
        return valid;
    }
    if (curr.val == wrapper.curr.val) {
        wrapper.curr = wrapper.curr.next;
        return true;
    } else {
        return false;
    }
}
```

### Solution 2

Another solution that I consider a genuine exercise is creating a reversed
copy of the list and then doing a straight up comparison.

The time complexity of this algorithm is `O(n)` where `n` is the length of
the list. The space complexity is also `O(n)`, necessary to hold a copy of
the list.

```java
boolean palindrome(Node head) {
    Node copy = reverseCopy(head);
    return equalLists(head, copy);
}

Node reverseCopy(Node head) {
    Node copy = new Node(null, head.val);
    head = head.next;
    while (head != null) {
        Node temp = new Node(copy, head.val);
        copy = temp;
        head = head.next;
    }
    return copy;
}

boolean equalLists(Node l1, Node l2) {
    while (l1 != null && l2 != null) {
        if (l1.val != l2.val) {
            return false;
        }
        l1 = l1.next;
        l2 = l2.next;
    }
    return l1 == null && l2 == null;
}
```

## 2.7 **Intersection:**

Given two (singly) linked lists, determine if the two lists intersect.
Return the intersecting node. Note that the intersection is defined based
on reference, not value. That is, if the kth node of the first linked list
is the exact same node (by reference) as the jth node of the second linked
list, then they are intersecting.

### Solution 1

Well doesn't a `Map` make this problem trivial? The time and space complexity
of this solution is `O(m + n)` where `m` and `n` are the lengths of the lists.

```java
Node intersectLists(Node n1, Node n2) {
    Set<Node> nodes = new HashSet<>();
    while (n1 != null) {
        nodes.add(n1);
    }
    while (n2 != null) {
        if (!nodes.add(n2)) {
            return n2;
        }
    }
    return null;
}
```

### Solution 2

Now obviously the authors of CTCI didn't think the above solution was very
clever. So how would you solve this without the simple `Map` solution?

If two lists interset they have the same last node. So this is easy to check.
Finding the intersection is also surprisingly easy if you know the trick.
The trick is to think if the lists were the same length we could just find
the intersection by advancing one node at a time in each list. We can exploit
this by "chopping off" the front of the longer list if we find that these
lists do in fact intersect.

The time complexity of this solution is `O(n)` where `n` is the length of the
longer list. The space complexity of this solution is `O(1)`.

```java
Node intersectLists(Node n1, Node n2) {
    NodeLength n1NodeLen = lastNode(n1);
    NodeLength n2NodeLen = lastNode(n2);
    if (n1NodeLen.node != n2NodeLen.node) {
        return null;
    }

    int diff = Math.abs(n1NodeLen.length - n2NodeLen.length);
    if (diff == 0) {
        return n1NodeLen.node;
    }

    Node longer = null;
    Node shorter = null;
    if (n1NodeLen.length > n2NodeLen.length) {
        longer = n1;
        shorter = n2;
    } else {
        longer = n2;
        shorter = n1;
    }

    while (diff > 0) {
        longer = longer.next;
        diff--;
    }

    while (longer != shorter) {
        longer = longer.next;
        shorter = shorter.next;
    }
    return longer;
}

class NodeLength {
    Node node;
    int length;
    NodeLength(Node node, int length) {
        this.node = node;
        this.length = length;
    }
}

NodeLength lastNode(Node node) {
    int length = 0;
    while (node.next != null) {
        node = node .next;
        length++;
    }
    return new NodeLength(node, length);
}
```

## 2.8 **Loop Detection:**

Given a circular linked list, implement an algorithm that returns the node
at the beginning of the loop.

### Solution 1

This could be implemented with a simple map that tracks when a node already
encountered is encountered the second time. Since this is so trivial, we
imagine this is not what the interviewer wants.

Instead we solve with the `slow` / `fast` pointer approach. The slow and fast
pointers will intersect in the loop. Say there are `k` nodes before the loop.
Then when the `slow` pointer enters the loop it will be `LOOP_SIZE % k` ahead
of the `fast` pointer. Since the `fast` pointer catches up 1 node in each
iteration it will catch up in `LOOP_SIZE % k` iterations. Now if we take one
of the pointers back to the start of the list, and advance until they intersect,
the intersection will take place at the start of the loop.

```java
Node findLoopStart(Node head) {
    Node fast = head;
    Node slow = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        if (fast == slow) {
            break;
        }
    }

    if (fast == null || fast.next == null) {
        return null;
    }

    slow = head;
    while (slow != fast) {
        slow = slow.next;
        fast = fast.next;
    }
    return slow;
}
```

## Stacks and Queues

## 3.1 **:**

Describe how you could use a single array to implement three stacks.

### Discussion

This is just ugly. You could mark out set ranges for each of the three stacks.
That is, a portion of the array for each stack and a bunch of instance variables
to record the starting positions and ending positions for each of them. I would
recommend the constructor take the number of stacks and the capacity of an
individual stack. The code is tedious but not difficult.

```java
public class MultiStack {
    final int num;
    final int capacity;
    final int[] values;
    final int[] sizes;

    public MultiStack(int num, int capacity) {
        this.num = num;
        this.capacity = capacity;
        this.values = new int[num * capacity];
        this.sizes = new int[num];
    }

    public void push(int stack, int val) {
        if (stack < 1 || stack > this.num) {
            throw new IllegalArgumentException("Indicated stack out of range: " + stack);
        }

        // zero based
        stack--;

        if (isFull(stack)) {
            throw new IllegalStateException("Stack already full: " + stack);
        }

        int offset = stack * capacity;
        this.values[offset + this.sizes[stack]] = val;
        this.sizes[stack]++;
    }

    public int pop(int stack) {
        return examine(stack, true);
    }

    public int peek(int stack) {
        return examine(stack, false);
    }

    private int examine(int stack, boolean remove) {
        if (stack < 1 || stack > this.num) {
            throw new IllegalArgumentException("Indicated stack out of range: " + stack);
        }

        // zero based
        stack--;

        if (isEmpty(stack)) {
            throw new IllegalStateException("Stack already empty: " + stack);
        }

        int offset = stack * capacity;
        int value = this.values[offset + this.sizes[stack] - 1];
        if (remove) {
            this.sizes[stack]--;
        }
        return value;
    }

    public boolean isFull(int stack) {
        return this.sizes[stack] == capacity;
    }

    public boolean isEmpty(int stack) {
        return this.sizes[stack] == 0;
    }
}
```

## 3.2 **:**

How would you design a stack which, in addition to push and pop, has a
function min which returns the minimum element? Push, pop and min should
all operate in `O(1)` time.

## Solution 1

Notice that it didn't say you need to be able to remove the `min` like a `pop`.
Thus the solution is to add a `min` field to your `StackNode` that keeps track
of the minimum in the stack up to that point. This keeps all the operations at
`O(1)`.

```java
class MinStack<T extends Comparable<T>> {
    class StackNode<T> {
        final T data;
        final T min;
        final StackNode<T> next;
        StackNode(T data, T min, StackNode<T> next) {
            this.data = data;
            this.min = min;
            this.next = next;
        }
    }

    StackNode<T> top = null;

    void push(T data) {
        if (this.top == null) {
            this.top = new StackNode<>(data, data, null);
        } else {
            T min = data.compareTo(this.top.data) < 0 ? data : this.top.data;
            StackNode<T> sn = new StackNode<>(data, min, this.top);
            this.top = sn;
        }
    }

    T peek() {
        if (this.top == null) {
            throw new IllegalStateException("Stack empty on peek.");
        }
        return top.data;
    }

    T pop() {
        if (this.top == null) {
            throw new IllegalStateException("Stack empty on pop.");
        }
        T data = top.data;
        top = top.next;
        return data;
    }

    T min() {
        if (this.top == null) {
            throw new IllegalStateException("Stack empty on min.");
        }
        return this.top.min;
    }

    boolean empty() {
        return this.top == null;
    }
}
```

## 3.3 **Stack of Plates:**

Imagine a (literal) stack of plates. If the stack gets too high, it might
opple. Therefore, in real life, we would likely start a new stack when the
previous statck exceeds some threshold. Implement a data structure
`SetOfStacks` that mimics this. `SetOfStacks` should be composed of several
stacks and should create a new stack once a previous one exceeds capacity.
`SetOfStacks.push()` and `SetOfStacks.pop()` should behave identically to a
single stack (that is `pop()` should return the same values as it would if
there were just a single stack).
Follow up: Implement a function `popAt(int)` which performs a pop operation
on a specific sub-stack.

### Solution 1

This straightforward solution using a `List` to hold a list of `Stack`s. Most
operations are similar to a regular stack. `pop()` that removes the last element
for a stack that is not the last stack, however, will require copying the other
stacks back down over the empty slot.

```java
public class SetOfStacks<T> {
    private static class Stack<T> {
        static class StackNode<T> {
            final T data;
            final StackNode<T> next;
            StackNode(T data, StackNode<T> next) {
                this.data = data;
                this.next = next;
            }
        }

        int size;
        StackNode<T> top;
        void push(T data) {
            if (this.top == null) {
                this.top = new StackNode<>(data, null);
            } else {
                StackNode<T> sn = new StackNode<>(data, this.top);
                this.top = sn;
            }
            size++;
        }

        T peek() {
            if (this.top == null) {
                throw new IllegalStateException("Empty stack on peek.");
            }
            return this.top.data;
        }

        T pop() {
            if (this.top == null) {
                throw new IllegalStateException("Empty stack on pop.");
            }
            T data = this.top.data;
            this.top = this.top.next;
            size--;
            return data;
        }

        int size() {
            return this.size;
        }
    }

    final int capacity;
    final List<Stack<T>> stacks = new ArrayList<>();
    public SetOfStacks(int capacity) {
        this.capacity = capacity;
    }

    void push(T data) {
        Stack<T> s = null;
        if (stacks.size() == 0) {
            s = new Stack<>();
            stacks.add(s);
        } else {
            s = stacks.get(stacks.size() - 1);
            if (s.size() == capacity) {
                s = new Stack<>();
                stacks.add(s);
            }
        }
        s.push(data);
    }

    T pop() {
        return popAt(stacks.size() - 1);
    }

    T popAt(int index) {
        if (index < 0 || index >= stacks.size()) {
            throw new IllegalArgumentException("Stacks index out of range: " + index);
        }
        Stack<T> s = stacks.get(index);
        T data = s.pop();
        if (s.size() == 0) {
            stacks.remove(index);
        }
        return data;
    }

    T peek() {
        if (stacks.size() == 0) {
            throw new IllegalStateException("Stacks empty on peek.");
        }
        return peekAt(stacks.size() - 1);
    }

    T peekAt(int index) {
        if (index < 0 || index >= stacks.size()) {
            throw new IllegalStateException("Index out of range on peek.");
        }
        Stack<T> s = stacks.get(index);
        return s.peek();
    }
}
```

## 3.4 **Queue Via Stacks:**

Implement a `MyQueue` class which implements a queue using two stacks.

### Solution 1

The main thing to get here is that a `Queue` can be implemented with two
stacks by keeping the values in a stack and when the first item is needed
you need to grab the bottom item. This can be done by pushing all the items
on another stack, and then working with the top item. This is obviously
expensive. To get the front item takes an `O(n)` shuffling of the data onto
another stack and back on the original.

```java
public class MyQueue<T> {
    Stack<T> stack = new Stack<>();
    Stack<T> aux = new Stack<>();

    void add(T data) {
        this.stack.push(data);
    }

    T remove() {
        return bottom(true);
    }

    T peek() {
        return bottom(false);
    }

    private T bottom(boolean remove) {
        if (this.stack.isEmpty()) {
            throw new IllegalStateException("Stack is empty.");
        }
        while (!this.stack.isEmpty()) {
            this.aux.add(this.stack.pop());
        }
        T data = remove ? this.aux.pop() : this.aux.peek();
        while (!this.aux.isEmpty()) {
            this.stack.add(this.aux.pop());
        }
        return data;
    }

    boolean isEmpty() {
        return this.stack.isEmpty();
    }
}
```

## 3.5 **Sort Stack:**

Write a program to sort a stack such that the smallest items are on the top.
You can use an additional temporary stack, but you may not copy the elements
into any other data structure (such as an array). The stack supports the
following operations: `push`, `pop`, `peek`, and `isEmpty`.

### Solution 1

We can sort using two stacks. Assume one of the stacks is sorted, and to help
kick things off note that an empty stack is sorted. Then in each round we pop
an item off the unsorted stack, call it `value`. Now we move `value` to its
correct position in the sorted stack. If the sorted stack is empty or `value`
is greater than the sorted stack's top, we can place it on the sorted stack.
Otherwise we pop down to the correct location and push `value` onto the sorted
stack. The items that were greater than `value` get pushed back on the unsorted
stack. Lastly we move the elements back into the source stack to get the desired
ordering.

```java
void sortStack(Stack<Integer> s) {
    Stack<Integer> sorted = new Stack<>();
    while (!s.isEmpty()) {
        int value = s.pop();
        while (!sorted.isEmpty() && value > sorted.peek()) {
            s.push(sorted.pop());
        }
        sorted.push(value);
    }

    while (!sorted.isEmpty()) {
        s.push(sorted.pop());
    }
}
```

## 3.6 **Animal Shelter:**

An animal shelter, which holds only dogs and cats, operates on a strictly
"first in, first out" basis. People must adopt either the "oldest" (based on
arrival time) of all animals at the shelter or they can select whether they
will receive a dog or a cat (and will receive the oldes animal of that
type). They cannot select which specific animal they would like. Create the
data strucutres to maintain this system and implement operations such as
`enqueue`, `dequeueAny`, `dequeueDog`, and `dequeueCat`. You may use the
built-in `LinkedList` data structure.

### Solution 1

Nothing fancy here. Just keep two `Dequeue`s, one for cats and one for dogs.
Enqueue them with an arrival stamp to when needing to choose the oldest amongst
both cats and dogs.

```java
public class AnimalShelter {
    static class Animal {
        final String name;
        final int arrival;
        Animal(String name, int arrival) {
            this.name = name;
            this.arrival = arrival;
        }
    }

    static class Cat extends Animal {
        Cat(String name, int arrival) {
            super(name, arrival);
        }
    }

    static class Dog extends Animal {
        Dog(String name, int arrival) {
            super(name, arrival);
        }
    }

    private final Deque<Dog> dogs = new LinkedList<>();
    private final Deque<Cat> cats = new LinkedList<>();
    private final AtomicInteger order = new AtomicInteger();

    void enqueuCat(String name) {
        this.cats.add(new Cat(name, order.getAndIncrement()));
    }

    void enqueuDog(String name) {
        this.dogs.add(new Dog(name, order.getAndIncrement()));
    }

    Cat dequeuCat() {
        if (this.cats.isEmpty()) {
            throw new IllegalStateException("No more cats.");
        }
        return this.cats.removeFirst();
    }

    Dog dequeuDog() {
        if (this.dogs.isEmpty()) {
            throw new IllegalStateException("No more dogs.");
        }
        return this.dogs.removeFirst();
    }

    Animal dequeuAny() {
        if (this.cats.isEmpty() && this.dogs.isEmpty()) {
            throw new IllegalStateException("No animals");
        } else if (!this.cats.isEmpty() && !this.dogs.isEmpty()) {
            Cat c = this.cats.peekFirst();
            Dog d = this.dogs.peekFirst();
            if (c.arrival < d.arrival) {
                return this.cats.removeFirst();
            } else {
                return this.dogs.removeFirst();
            }
        } else if (this.cats.isEmpty()) {
            return this.dogs.removeFirst();
        } else {
            return this.cats.removeFirst();
        }
    }
}
```

# Trees and Graphs

## 4.1 **Route Between Nodes:**

Given a directed graph, design an algorithm to find out whether there is a
route between two nodes.

### Solution 1

A BFS approach should do the trick with a `Set` to track already visited nodes.
Note that as the graph is directed, we start a search from each node to check
if there is a route in either direction.

The time complexity of this solution is `O(n)` where `n` is the number of nodes
in the graph. This comes from the possible need to visit every node through the
traversal. Likewise, the space complexity is also `O(n)` to accomodate adding
the nodes to the `enqueued` set.

```java
boolean route(Node<?> n1, Node<?> n2) {
    return directedRoute(n1, n2) || directedRoute(n2, n1);
}

boolean directedRoute(Node<?> n1, Node<?> n2) {
    Set<Node<?>> enqueued = new HashSet<>();
    Queue<Node<?>> q = new LinkedList<>();
    q.add(n1);
    enqueued.add(n1);
    while (!q.isEmpty()) {
        Node<?> n = q.remove();
        if (n == n2) {
            return true;
        }
        for (Node<?> c : n.neighbors) {
            if (!enqueued.contains(c)) {
                q.add(c);
                enqueued.add(c);
            }
        }
    }
    return false;
}
```

## 4.2 **Minimal Tree:**

Given a sorted (increasing order) array with unique integer elements, write
an algorithm to create a binary search tree with minimal height.

### Solution 1

This can be done with a recursive solution that makes the middle element the
root of a tree and then recurses around that middle element for the left and
right subtrees.

The time complexity of this solution is `O(n)` where `n` is the number of
elements in the array. The comes from invoking the recursive function for
the creation of each `TreeNode`. The space complexity of this solution is
`O(d)` where `d` is the depth of the resultant tree. This would be the
same as `O(log(n))` as the tree will end up relatively balanced.

```java
<T> TreeNode<T> createTree(T[] arr) {
    return createTree(arr, 0, arr.length - 1);
}

<T> TreeNode<T> createTree(T[] arr, int lo, int hi) {
    if (lo > hi) {
        return null;
    }
    int mid = lo + (hi - lo) / 2;
    TreeNode<T> node = new TreeNode<>(arr[mid], null, null);
    node.left = createTree(arr, 0, mid - 1);
    node.right = createTree(arr, mid + 1, hi);
    return node;
}
```

## 4.3 **List of Depths:**

Given a binary tree, design an algorithm which creates a linked list of all
the nodes at each depth.

### Solution 1

Whenever you see a question asking you to process a tree by level you should
first think of a BFS traversal solution. With the double loop trick (see the
usage of the `size` variable), this type of problem becomes trivial.

The time complexity of this solution is `O(n)` where `n` is the number of
elements in the tree.  The space complexity question is a little different.
The data structure holding the solution has to have the `n` nodes in it so
it is `O(n)`. In the processing, however, the space used is the maximum size
of the `Queue` used to hold the BFS solution. This would be the size of the
largest level. Given a tree with depth `d` the largest level would have the
size of `2^d - 1`. I suppose this makes the space complexity `O(2^d)`.

```java
<T> List<List<TreeNode<T>>> extractLevels(TreeNode<T> root) {
    List<List<TreeNode<T>>> levels = new ArrayList<>();
    Queue<TreeNode<T>> q = new LinkedList<>();
    q.add(root);
    int level = 0;
    while (!q.isEmpty()) {
        int size = q.size();
        while (size-- > 0) {
            TreeNode<T> tn = q.remove();
            if (level == levels.size()) {
                levels.add(new ArrayList<>());
            }
            levels.get(level).add(tn);
        }
    }
    return levels;
}
```

## 4.4 **Check Balanced:**

Implement a function to check if a binary tree is balanced. For the purposes
of this question, a balanced tree is defined to be a tree such that the
heights of the two subtrees of any one node never differ by more than one.

### Solution 1

This solution uses a recursive approach to get the height of the left and right
subtrees to percolate back up to each node. A special value, a height of `-1`
is used to indicate that the tree is not balanced and further checking is
unnecessary.

The time complexity of this solution is `O(n)` where `n` is the number of nodes
in the tree. The space complexity, due to the use of recursion, is the depth of
the tree. In the worst case this may be every node so it is also `O(n)`.

```java
<T> boolean balanced(TreeNode<T> node) {
        return getTreeHeight(node) != -1;
}

<T> int getTreeHeight(TreeNode<T> node) {
    if (node == null) {
        return 0;
    }
    int lheight = getTreeHeight(node.left);
    int rheight = getTreeHeight(node.right);
    if (lheight == -1 || rheight == -1) {
        return -1;
    }
    int diff = Math.abs(lheight - rheight);
    if (diff > 1) {
        return -1;
    }
    return Math.max(lheight, rheight) + 1;
}
```

## 4.5 **Validate BST:**

Implement a function to check if a binary tree is a binary search tree.

### Solution 1

One obvious way to do this that also will exercise your tree traversal skills
is to realize that the accumulation of an in order traversal can be used to
verify a BST. That is if the in order traversal results in an in order
accumulated list then the tree is in fact a BST.

The time complexity of this solution is `O(n)` where `n` is the number of nodes
in the tree. This comes from the nature of the in order traversal and visiting
each node. The space complexity of this solution is also `O(n)` as the
accumulator obviously has to hold every node in the tree.


```java
boolean isBst(TreeNode root) {
    List<TreeNode> acc = new ArrayList<>();
    traverse(root, acc);
    for (int i = 1; i < acc.size(); i++) {
        if (acc.get(i).val < acc.get(i - 1).val) {
            return false;
        }
    }
    return true;
}

void traverse(TreeNode node, List<TreeNode> acc) {
    if (node == null) {
        return;
    }
    traverse(node.left, acc);
    acc.add(node);
    traverse(node.right, acc);
}
```

### Solution 2

A cooler way to do this is to use what I will refer to here as "fencing". That
is we use recursion and we pass a "fence" down the recursive calls that
specifies the lower bound and upper bound `(lo, hi)` that all nodes from a
paricular node must be between. How do you define the fence? When going left
from node `n`, all descendant nodes must be between `(lo, n.val)`. When going
right from node `n`, all descendant nodes must be between `(n.val, hi)`.

The time complexity of this solution is `O(n)` where `n` is the number of nodes
in the tree. The space complexity is also `O(n)`. This comes from the recursive
calls and the case where the tree doesn't branch. If the tree is balanced, the
space complexity is `O(d)` where `d` is the depth of the tree which is
`O(log(n))`.

Note that this solution assumes the tree doesn't allow for equivalent values.
Also note that `Integer.MIN_VALUE` and `Integer.MAX_VALUE` can't be a part of
the tree.

```java
boolean isBst(TreeNode root) {
    if (root == null) {
        return true;
    }
    return isBst(root.left, Integer.MIN_VALUE, root.val) &&
            isBst(root.right, root.val, Integer.MAX_VALUE);
}

boolean isBst(TreeNode node, int lo, int hi) {
    if (node == null) {
        return true;
    }
    return node.val > lo && node.val < hi &&
            isBst(node.left, lo, node.val) &&
            isBst(node.right, node.val, hi);
}
```

### Solution 3

This solution is a minor variation on the prior. Here we use `Integer`
objects to side step the `Integer.MIN_VALUE`, `Integer.MAX_VALUE` problem
left in the prior solution by using `null` to indicate no limit.

This solution has the same time and space complexity as the prior solution.
In practice, the boxing / unboxing of `Integer <-> int` here would be
interesting to profile compared to the prior solution.

```java
boolean isBst(TreeNode root) {
    if (root == null) {
        return true;
    }

    return isBst(root.left, null, root.val) &&
            isBst(root.right, root.val, null);
}

boolean isBst(TreeNode node, Integer lo, Integer hi) {
    if (node == null) {
        return true;
    }
    if (lo != null && node.val < lo) {
        return false;
    }
    if (hi != null && node.val > hi) {
        return false;
    }
    return isBst(node.left, lo, node.val) && isBst(node.right, node.val, hi);
}
```

## 4.6 **Successor:**

Write an algorithm to find the "next" node (in order successor) of a given
node in a binary search tree. You may assume each node has a link to its
parent.

### Solution 1

A simplistic solution merely accumulates the in order traversal and then looks
for the node in question. Note that the problem description doesn't say you
will be given the root node. However, if you have any node you can walk the tree
back to the root.

The time and space complexity of this solution is `O(n)` where `n` is the number
of nodes in the tree. The time complexity comes from visiting each node and then
going through the accumulated node list. The space complexity comes from the
recursion and the accumulated node list.

```java
TreeNode successor(TreeNode node) {
    List<TreeNode> acc = new ArrayList<>();
    successor(findRoot(node), acc);
    int i = 0;
    while (acc.get(i) != node) {
        i++;
    }
    return i < acc.size() - 1 ? acc.get(i + 1) : null;
}

TreeNode findRoot(TreeNode node) {
    while (node.parent != null) {
        node = node.parent;
    }
    return node;
}

void successor(TreeNode node, List<TreeNode> acc) {
    if (node == null) {
        return;
    }
    successor(node.left, acc);
    acc.add(node);
    successor(node.right, acc);
}
```

### Solution 2

There is a much simpler way to do this problem if we think of how an in order
accumulating traversal works. If instead of passing a data structure that
accumlates the traversal we pass something that indicates when the "last"
accumulated node was the node in question, we can then return the successor.
Note that we need the `findRoot` method above for this approach.

This solution has time complexity `O(n)` where `n` is the number of nodes in
the tree. This comes from finding the root and also potentially visiting all
the nodes in the tree during accumulation. The space complexity is also `O(n)`,
coming from both the recursive stack frames and the accumulating list.

```java
TreeNode successor(TreeNode node) {
  return successort(node, findRoot(node), new boolean[1]);
}

TreeNode findRoot(TreeNode node) {
    while (node.parent != null) {
        node = node.parent;
    }
}

TreeNode successor(TreeNode n1, TreeNode curr, boolean[] last) {
    if (curr == null) {
        return null;
    }

    TreeNode result = successor2(n1, curr.left, last);
    if (result != null) {
        return result;
    }
    if (last[0]) {
        return curr;
    }
    if (curr == n1) {
        last[0] = true;
    }
    return successor2(n1, curr.right, last);
}
```

### Solution 3

This is the "big boy" solution for this problem. Here we almost certainly won't
be recursing on the whole tree. Likewise, we don't have to start with finding
the root node. The solution works by recognizing that the successor node will
either be the leftmost node in the right subtree, or the first "left node" up
the tree.

The time complexity remains `O(n)` in the worst as you may need to visit all
the nodes in the tree. The space complexity, however, is `O(1)`.

```java
TreeNode successor1(TreeNode node) {
    if (node == null) {
        return null;
    }

    if (node.right != null) {
        return leftMost(node.right);
    } else {
        TreeNode parent = node.parent;
        while (parent != null && parent.left != node) {
            node = parent;
            parent = parent.parent;
        }
        return node;
    }
}

TreeNode leftMost(TreeNode node) {
    while (node.left != null) {
        node = node.left;
    }
    return node;
}
```

## 4.7 **Builder Order:**

You are given a list of projects and a list of dependencies (which is a list
of pairs of projects, where the second project is dependent on the first
project). All of a project's dependencies must be built before the project
is. Find a build order that will allow the projects to be built. If there is
no valid build order, return an error.

### Solution 1

We solve this problem by maintaining two maps. The first map, `deps`, is from
a project to the projects it is dependent on. The second map, `locs`, is from
a project to the projects that are dependent on it. In each round we take a
project that is "clear" (`deps` list is empty) and add it to the build order.
Since it is clear, we can remove it as a dependency from all other projects.
If this takes that project's dependencies to zero then we add it to `clear`
to be processed in a later round. At the end of each round we remove the
"clear" projects from `deps`. When `deps` or `clear` become empty we are
finished. If `deps` is empty we have an acceptable build order. If `deps` is
not empty there was no way to satisfy the build order.

```java
List<String> buildOrder(List<String> projects, List<List<String>> dependencies) {
    // Map from project to projects it is dependent on.
    Map<String, Set<String>> deps = new HashMap<>();
    // Map from project to projects dependent on it.
    Map<String, Set<String>> locs = new HashMap<>();
    for (String project : projects) {
        deps.put(project, new HashSet<>());
        locs.put(project, new HashSet<>());
    }
    // Projects that are "clear" to execute, all dependencies satisfied.
    Set<String> clear = new HashSet<>(projects);
    for (List<String> d : dependencies) {
        String dependent = d.get(1);
        String on = d.get(0);
        deps.get(dependent).add(on);
        clear.remove(dependent);
        locs.get(on).add(dependent);
    }
    List<String> order = new ArrayList<>();
    while (!deps.isEmpty() && !clear.isEmpty()) {
        String project = clear.iterator().next();
        order.add(project);
        for (String p : locs.get(project)) {
            Set<String> d = deps.get(p);
            d.remove(p);
            if (d.isEmpty()) {
                clear.add(p);
            }
        }
        deps.remove(project);
        clear.remove(project);
    }
    return deps.isEmpty() ? order : null;
}
```

## 4.8 **First Common Ancestor:**

Design an algorithm and write code to find the first common ancestor of two
nodes in a binary tree. Avoid storing additional nodes in a data structure.
NOTE: This is not necessarily a binary search tree.

### Solution 1

Note that they up front say "avoid storing additional nodes in a data
structure." This eliminates a solution that stores the paths down to `p` and
`q` and then determines where these paths diverge. The last node before the
divergence is the answer.

We can use a recursive approach to do this. In this solution we use a `Finds`
return value to indicate if the common node has been found. It does this by
tracking if the `p` and `q` values have been found. When both are first found
we set `common` to the common node. If `common` is found, in a sub-invocation,
then the current invocation can merely pass back up that response. Note that
this implementation avoids creating new `Find` instances by reusing the
instances passed up through recursive calls. The code might be slightly more
clear (but slower?) if it just created new `Find` instances.

The time complexity of this solution is `O(n)` where `n` is the number of nodes
in the tree. This comes from the potential need to visit every node to find
`p` and `q` or to find that one or both is missing. The space complexity is
also `O(n)`. This comes from the recursive calls and the possibility that the
tree does not branch.

```java
class Finds {
    TreeNode common;
    boolean first;
    boolean second;
}

class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        Finds f = finds(root, p, q);
        return f.common;
    }

    private Finds finds(TreeNode node, TreeNode p, TreeNode q) {
        if (node == null) {
            return new Finds();
        }

        Finds left = finds(node.left, p, q);
        if (left.common != null) {
            return left;
        }
        Finds right = finds(node.right, p, q);
        if (right.common != null) {
            return right;
        }

        if ((left.first && right.second) || (left.second && right.first)) {
            left.first = left.second = true;
            left.common = node;
            return left;
        }

        if (node == p) {
            left.first = true;
            left.second |= right.second;
            if (left.second) {
                left.common = node;
            }
            return left;
        }
        if (node == q) {
            left.first |= right.first;
            left.second = true;
            if (left.first) {
                left.common = node;
            }
            return left;
        }
        left.first |= right.first;
        left.second |= right.second;
        return left;
    }
}
```

### Solution 2

If we assume that both `p` and `q` exist in the tree, which the wording in the
problem description seems to imply, we can greatly simplify this code. We do
this by merely having the recursive return `p` and `q` if found. If a non null
is returned from the left and the right, then that parent invocation must be
the common node. If a non null is only returned from one side, then that
returned node must be the parent of both nodes that is percolating up the tree.

The time and space complexity remain `O(n)`.

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null) return null;
        if (root == p || root == q) return root;
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        if (left != null && right != null) {
            return root;
        } else if (left != null) {
            return left;
        } else {
            return right;
        }
    }
}
```

## 4.9 **BST Sequences:**

A binary search tree was created by traversing through an array from left
to right and inserting each element. Given a binary search tree with
distinct elements, print all possible arrays that could have led to this
tree.

### Solution 1

This is a hard problem. Thinking about an example tree, the root would have
to be the first element in the array. After that, positions are dictated by
their ordering with respect to the root. Smaller items go to the left, larger
items to the right. What do we have to work with here? Well imagine we already
have the solution for traveling through `node.left` and `node.right`. Then
these solutions can be "woven" together with `node` at the front of the array
and all weavings of the left and right that keep the respective orders in tact.
The implementation involves two recursive methods where the inner method weaves
the lists.

```java
List<LinkedList<Integer>> allSequences(TreeNode node) {
    List<LinkedList<Integer>> results = new ArrayList<>();
    if (node == null) {
        results.add(new LinkedList<>());
        return results;
    }

    LinkedList<Integer> prefix = new LinkedList<>();
    prefix.add(node.val);

    List<LinkedList<Integer>> left = allSequences(node.left);
    List<LinkedList<Integer>> right = allSequences(node.right);

    for (LinkedList<Integer> l : left) {
        for (LinkedList<Integer> r : right) {
            List<List<Integer>> weaved = new ArrayList<>();
            weaveLists(l, r, prefix, weaved);
        }
    }

    return results;
}

void weaveLists(LinkedList<Integer> first, LinkedList<Integer> second,
        LinkedList<Integer> prefix, List<List<Integer>> results) {
    if (first.isEmpty() || second.isEmpty()) {
        List<Integer> result = new ArrayList<>();
        result.addAll(prefix);
        result.addAll(first);
        result.addAll(second);
        results.add(result);
    }

    prefix.add(first.removeFirst());
    weaveLists(first, second, prefix, results);
    first.addFirst(prefix.removeLast());

    prefix.add(second.removeFirst());
    weaveLists(first, second, prefix, results);
    second.addFirst(prefix.removeLast());
}
```

## 4.10 **Check Subtree:**

`T1` and `T2` are two very large binary trees, with `T1` much bigger than
`T2`. Create an algorithm to determine if `T2` is a subtree of `T1`.

A tree `T2` is a subtree of `T1` if there exists a node `n` in `T1` such
that the subtree of `n` is identical to `T2`. That is, if you cut off the
tree at node `n`, the two trees would be identical.

### Solution 1

A recursive solution works nicely here. We traverse `haystack` and whenever
we find a node matching the root of `needle` we being a tree matching
function.

The time complexity of this solotion is `O(n*m)` where `n` is the number of
nodes in `haystack` and `m` is the number of nodes in `needle`. The space
complexity is `O(n)`. This comes from the recursive calls to iterate
`haystack`.

```java
boolean checkSubtree(TreeNode needle, TreeNode haystack) {
    if (needle == null) {
        return true;
    }
    if (haystack == null) {
        return false;
    }
    boolean subtree = needle.val == haystack.val &&
            checkSubtree(needle.left, haystack.left) &&
            checkSubtree(needle.right, haystack.right);
    if (subtree) {
        return subtree;
    }
    return checkSubtree(needle, haystack.left) || checkSubtree(needle, haystack.right);
}
```

## 4.11 **Random Node:**

You are implemented a binary tree class from scratch which, in addition to
insert, find, and delete, has a method `getRandomNode()` which returns a
random node from the tree. All nodes should be equally likely to be chosen.
Design and implement an algorithm for `getRandomNode()`, and explain how you
would implement the rest of the methods.

### Solution 1

A rather clever problem indeed. Let's say we have implemented a tree class that
keeps track of how many descendants are in the tree beneath it. It does this by
augmenting the standard `TreeNode` class with `leftCount` and `rightCount`
instance variables. This is not hard to do. When adding a node the traversal to
the correct spot can increment the counts as it goes. Likewise with removing a
node. The counts can be corrected going back to the root. For a balanced binary
tree this may be a bit more involved, but I haven't throught this fully through
yet.

With these counts in place, choosing a random node is rather straightforward. We
choose a random number in the range of the total node count. To make the
selection random, we navigate per the value of this number. Say the root node is
at position `x` with `l` nodes to the left of it and `r` nodes to the right. If
the random value generated, `pos`, is `x`, we return the root node. If it is
less than `x` we recurse to the left, greater than `x` we recurse to the right.
When recursing to the left, no adjustment in the `pos` value is needed. When
recursing to the right we reduce `pos` accordingly subtracting the value equal
to the count in the left subtree.

The time complexity of this solution is `O(d)` where `d` is the depth of the
tree. For a balanced tree this is `O(log(n))`. The space complexity is the same,
as the solution uses recursion to possibly travel to the bottom of the tree.

```java
TreeNode randomNode(TreeNode node) {
    if (node == null) {
        return null;
    }
    int count = 1 + node.leftCount + node.rightCount;
    int pos = new Random().nextInt(count);
    return node(node, pos);
}

TreeNode node(TreeNode node, int pos) {
    if (pos == node.leftCount) {
        return node;
    } else if (pos < node.leftCount) {
        return node(node.left, pos);
    } else {
        return node(node.right, pos - node.leftCount);
    }
}
```

## 4.12 **Paths with Sum:**

You are given a binary tree in which each node contains an integer value (which
might be positive or negative). Design an algorithm to count the number of paths
that sum to a given value. The path does not need to start or end at the root or
a leaf, but it must go downwards (traveling only from parent nodes to child
nodes).

### Solution 1

A brute for solution does a traversal within a traversal. The second traversal
stores the current path sum and increments a counter each time it finds a match.
This is highly inefficient. First, it visits all `n` nodes, and from each node
it does a full traversal of that subtree. Each one of these subtraversals is
capped at a max of `n` nodes making this `O(n^2)`. Ok, it is polynomial. If the
tree is balanced, it isn't quite as bad, as the number of nodes from each
traversal is:

`n + (n - 1) + (n - 2) + (n - 4) + ... (n - n)`

The number of terms would be the depth of the tree, which is a balanced tree
would be `O(log(n))`.  The second term in each grouping is a power of two.
The sum of the powers of two from `1` to `n` is `n`. Thus the runtime in the
case of a balanced tree is `O(n * log(n))`.

To beat this, we use a map to track all the prior prefixes seen on the current
path. We can match the target sum when the current path equals that target or
the current path minus a prefix equals that target sum. The code is
surprisingly concise.

The time complexity of this solution is `O(n)` where `n` is the number of nodes
in the tree. The space complexity is also `O(n)`, however, in the case of a
balanced binary tree the space complexity is `O(log(n))`. This comes from the
recursive stack frames and the map holding the prefixes.

```java
int pathSums(TreeNode root, int target) {
    return pathSums(root, target, 0, new HashMap<>());
}

int pathSums(TreeNode node, int target, int curr, Map<Integer, Integer> prefixes) {
    if (node == null) {
        return 0;
    }

    curr = curr + node.val;
    int counts = prefixes.getOrDefault(curr - target, 0);
    if (curr == target) {
        counts++;
    }

    prefixes.put(curr, prefixes.getOrDefault(curr, 0) + 1);
    counts += pathSums(node.left, target, curr, prefixes);
    counts += pathSums(node.right, target, curr, prefixes);
    prefixes.put(curr, prefixes.get(curr) - 1);
    return counts;
}
```
