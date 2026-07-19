# Delete All Occurrences of a Key in Doubly Linked List

---

# Problem Statement

Given the head of a **Doubly Linked List (DLL)** and an integer `key`, delete **all nodes** whose value is equal to `key`.

Return the head of the modified linked list.

---

# Key Observation ⭐

Unlike a Singly Linked List,

a Doubly Linked List has

```
Prev Pointer

+

Next Pointer
```

Whenever a node is deleted,

both pointers must be updated correctly.

---

# Approach : Traverse and Delete (Optimal)

## Intuition ⭐⭐⭐

Traverse the DLL node by node.

Whenever the current node's value equals the key,

1. Store the next node.
2. Connect the previous node to the next node.
3. Connect the next node to the previous node.
4. If deleting the head,
   update the head.
5. Delete the current node.
6. Continue traversal.

---

# Different Cases

### Case 1 : Delete Head

Before

```
NULL <- 10 <-> 20 <-> 30
```

After

```
NULL <- 20 <-> 30
```

Update

```
head = head->next
```

---

### Case 2 : Delete Middle Node

Before

```
1 <-> 2 <-> 3 <-> 4
```

Delete

```
3
```

After

```
1 <-------> 2 <-------> 4
```

---

### Case 3 : Delete Tail

Before

```
1 <-> 2 <-> 3
```

Delete

```
3
```

After

```
1 <-> 2
```

---

# Algorithm

1. Traverse the DLL.
2. If current node is not the key,
   move ahead.
3. Otherwise,
   - Save the next node.
   - Update previous node's `next`.
   - Update next node's `prev`.
   - Update head if needed.
   - Delete current node.
4. Continue traversal.
5. Return head.

---

# Code

```cpp
class Solution {
public:
    Node* deleteAllOccurOfX(Node* head, int x) {

        Node* temp = head;

        while(temp){

            if(temp->data == x){

                Node* nextNode = temp->next;
                Node* prevNode = temp->prev;

                if(prevNode)
                    prevNode->next = nextNode;
                else
                    head = nextNode;

                if(nextNode)
                    nextNode->prev = prevNode;

                delete temp;

                temp = nextNode;
            }
            else{

                temp = temp->next;
            }
        }

        return head;
    }
};
```

---

# Time Complexity

```
O(N)
```

Every node is visited once.

---

# Space Complexity

```
O(1)
```

Only a few pointers are used.

---

# Why Save the Next Node?

Before deleting the current node,

store

```cpp
Node* nextNode = temp->next;
```

After

```cpp
delete temp;
```

the pointer becomes invalid.

Using `nextNode` allows us to continue traversing safely.

---