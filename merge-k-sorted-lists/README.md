# Merge k Sorted Lists

You are given an array of `k` linked-lists `lists`, each linked-list is sorted
in ascending order.

Merge all the linked-lists into one sorted linked-list and return it.

## Hints

1. This is just the typical merge of `2` sorted lists take to `k` sorted lists.
   A very similar approach applies. I don't think this should be classified as
   a leeter "hard" problem.

## Solutions

### Typical merge procedure

This solution follows the typical merging procedure. That is, we look for the
ListNode with the lowest value and store the index to that item in the list.
At the end of the loop we put that node on our merged list, and advance the
source list position to its next element. In subsequent loopings when we see
a null we know that list is already exhuasted and just hop to the next list.
Eventually all lists are exhausted and the sorted merged list is complete.

This solution does not fair well in the leeter speed ranking. The time
complexity here is O(m * n) where m is the number of lists and n is the total
number of nodes in the lists. The space complexity is O(1) as we allocate only
constant space in processing of the lists and reuse the source ListNodes
themselves to formulate the merged list.

```java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        ListNode head = null;
        ListNode curr = null;
        for (;;) {
            int minVal = Integer.MAX_VALUE;
            int minIdx = -1;
            for (int i = 0; i < lists.length; i++) {
                ListNode ln = lists[i];
                if (ln != null && ln.val < minVal) {
                    minVal = ln.val;
                    minIdx = i;
                }
            }
            if (minIdx == -1) {
                break;
            }
            ListNode ln = lists[minIdx];
            if (head == null) {
                head = curr = ln;
            } else {
                curr.next = ln;
                curr = ln;
            }
            lists[minIdx] = ln.next;
        }
        return head;
    }
}
```

### Merge one at a time

This solution merges in pairs at a time, much like the typical merge two sorted
lists problems. It is faster than the above solution in practice but is still not
a good performer.

The time complexity of this solution is O(m * n) where m is the number of lists
and n is the total number of nodes. The space complexity is O(1) as we merge
the lists in place.

```java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        if (lists.length == 0) {
            return null;
        }
        if (lists.length == 1) {
            return lists[0];
        }
        ListNode head = null;
        for (int i = 0; i < lists.length; i++) {
            head = merge(head, lists[i]);
        }
        return head;
    }

    private ListNode merge(ListNode l1, ListNode l2) {
        ListNode head = null;
        ListNode curr = null;
        for (;;) {
            if (l1 == null && l2 == null) {
                break;
            } else if (l2 == null) {
                if (head == null) {
                    head = curr = l1;
                } else {
                    curr.next = l1;
                    curr = l1;
                }
                l1 = l1.next;
            } else if (l1 == null) {
                if (head == null) {
                    head = curr = l2;
                } else {
                    curr.next = l2;
                    curr = l2;
                }
                l2 = l2.next;
            } else {
                if (l1.val < l2.val) {
                    if (head == null) {
                        head = curr = l1;
                    } else {
                        curr.next = l1;
                        curr = l1;
                    }
                    l1 = l1.next;
                } else {
                    if (head == null) {
                        head = curr = l2;
                    } else {
                        curr.next = l2;
                        curr = l2;
                    }
                    l2 = l2.next;
                }
            }
        }
        return head;
    }
}
```

### Merge pairs

Here instead of merging one list in at a time we merge pairs of lists and then
repeated merge the resultant pairs until there is only one list left.

This reduces the time complexity to O(log(m) * n) where m is the number of lists
and n is the total number of nodes. That is up to n nodes are merged in each
merging but in doing them in pairs there will only be log(m)
(technically base 2) mergings. This is a divide and conquer approach.

The space complexity for this implementation is O(m), as we construct a list
to hold the merged pairs as we whittle them down to one list. This could be
improved by merging these pairs into slots of the passed in `lists` to make
this O(1).

On the leeter the speed improvement over the prior solutions is significant.

```java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        if (lists.length == 0) {
            return null;
        }
        if (lists.length == 1) {
            return lists[0];
        }
        List<ListNode> a = Arrays.asList(lists);
        while (a.size() > 1) {
            List<ListNode> b = new ArrayList<>();
            for (int i = 0; i < a.size(); i += 2) {
                ListNode l1 = a.get(i);
                ListNode l2 = i < a.size() - 1 ? a.get(i + 1) : null;
                ListNode head = merge(l1, l2);
                b.add(head);
            }
            a = b;
        }
        return a.get(0);
    }

    private ListNode merge(ListNode l1, ListNode l2) {
        ListNode head = null;
        ListNode curr = null;
        for (;;) {
            if (l1 == null && l2 == null) {
                break;
            } else if (l2 == null) {
                if (head == null) {
                    head = curr = l1;
                } else {
                    curr.next = l1;
                    curr = l1;
                }
                l1 = l1.next;
            } else if (l1 == null) {
                if (head == null) {
                    head = curr = l2;
                } else {
                    curr.next = l2;
                    curr = l2;
                }
                l2 = l2.next;
            } else {
                if (l1.val < l2.val) {
                    if (head == null) {
                        head = curr = l1;
                    } else {
                        curr.next = l1;
                        curr = l1;
                    }
                    l1 = l1.next;
                } else {
                    if (head == null) {
                        head = curr = l2;
                    } else {
                        curr.next = l2;
                        curr = l2;
                    }
                    l2 = l2.next;
                }
            }
        }
        return head;
    }
}
```

### Merge pairs in place

Ah you couldn't leave well enough alone could ya? Here we do the same merge
pairs as above but we use the passed in `lists` to hold the intermediate
results. This reduces the space complexity to O(1).

```java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        if (lists.length == 0) {
            return null;
        }
        if (lists.length == 1) {
            return lists[0];
        }
        int size = lists.length;
        while (size > 1) {
            int fill = 0;
            for (int i = 0; i < size; i += 2) {
                ListNode l1 = lists[i];
                ListNode l2 = i < size - 1 ? lists[i + 1] : null;
                ListNode head = merge(l1, l2);
                lists[fill++] = head;
            }
            size = fill;
        }
        return lists[0];
    }

    private ListNode merge(ListNode l1, ListNode l2) {
        ListNode head = null;
        ListNode curr = null;
        for (;;) {
            if (l1 == null && l2 == null) {
                break;
            } else if (l2 == null) {
                if (head == null) {
                    head = curr = l1;
                } else {
                    curr.next = l1;
                    curr = l1;
                }
                l1 = l1.next;
            } else if (l1 == null) {
                if (head == null) {
                    head = curr = l2;
                } else {
                    curr.next = l2;
                    curr = l2;
                }
                l2 = l2.next;
            } else {
                if (l1.val < l2.val) {
                    if (head == null) {
                        head = curr = l1;
                    } else {
                        curr.next = l1;
                        curr = l1;
                    }
                    l1 = l1.next;
                } else {
                    if (head == null) {
                        head = curr = l2;
                    } else {
                        curr.next = l2;
                        curr = l2;
                    }
                    l2 = l2.next;
                }
            }
        }
        return head;
    }
}
```
