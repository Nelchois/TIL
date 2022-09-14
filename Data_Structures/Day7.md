## Linked List
There are two types of linked list, one_way and bidirectional.

### One_way linked list
In one_way linked list doesn't allocate linked memory, so each element has pair value that Key and link(it may or may not have value), We call that 'node'.
The first node is 'head node'.
If we want specific value, we should follow from head node to specific node. 
So, this sequence take more time than array. However if we change specific value, we just link with before and after node.(assume that we already know about where before and after node is.) It take only O(1) time complexity.

```
class Node:
    def __init__(self, key = None):
        self.key = key
        self.next = None
    def __str__(self):
        return str(self.key)

a = Node(3)
b = Node(9)
c = Node(-1)
a.next = b
b.next = c
```
This code is basic structure of one_way linked list, however is uncomfortable that allocate all variable each node.

```
class Node:
    def __init__(self, key = None):
        self.key = key
        self.next = None

    def __str__(self):
        return str(self.key)

class simply_Linked_list:
    def __init__(self):
        self.head = None
        self.size = 0

    def pushFront(self, key):
        new_node = Node(key)
        new_node.next = L.head
        L.head = new_node
        L.size += 1

    def pushBack(self, key):
        v = Node(key)
        if len(self) ==0: 
            self.head = v
        else: 
            tail = self.head
            while tail.next != None:
                tail = tail.next
            tail.next = v
        self.size += 1
    def __len__(self):
        return self.size

```
### Bidirectional linked list
Bidriectional linked list has 2 links in each node.