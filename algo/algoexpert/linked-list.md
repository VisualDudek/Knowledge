# Linked List

## Construction

main issue is the smart order of implementig methods

## Remove Kth Node From End

Singly Linked List

* use two pointers, first and second such way that second is ahead of first by k nodes
* edge case: if you need to remove first node, it is done differently 
* stop condition second.next NOT second bc you need reference to the node before node that will be removed 

```text
# pseudo-code

e.g. for k = 4
F                   S             
0 -> 1 -> 2 -> 3 -> 4 -> None

1) you want second pointer of k-ahead of first node, not at k-th node
check the edge case: if second is None -> you need to remove first node

iteruj po first and second till second.next is None
# implicite first points to the node before node that will be removed
first.next = first.next.next
```

