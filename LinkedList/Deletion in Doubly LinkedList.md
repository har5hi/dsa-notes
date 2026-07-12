# Deletion in Doubly Linked List (C++) 🗑️

> Deletion in a **Doubly Linked List (DLL)** is easier than in a Singly Linked List because every node has both:
>
> - `next` → points to the next node
> - `back` → points to the previous node
>
> During deletion, we simply reconnect the neighboring nodes and free the memory of the deleted node.

---

# Node Structure

```cpp
class Node {
public:
    int data;
    Node* next;
    Node* back;

    Node(int data1) {
        data = data1;
        next = nullptr;
        back = nullptr;
    }

    Node(int data1, Node* next1, Node* back1) {
        data = data1;
        next = next1;
        back = back1;
    }
};
```

---

# Visual Representation

```
NULL <- 10 <-> 20 <-> 30 <-> 40 -> NULL
```

Each node stores

```
+---------------------------+
| back | data | next |
+---------------------------+
```

---

# Pointer Rules ⭐

Whenever deleting a node,

```
prev <-> deleteNode <-> front
```

becomes

```
prev <---------------> front
```

After reconnecting the pointers,

```
delete deleteNode;
```

---

# 1. Delete Head

## Code

```cpp
Node* deleteHead(Node* head){

    // Empty list
    if(head == nullptr){
        return nullptr;
    }

    // Only one node
    if(head->next == nullptr){
        delete head;
        return nullptr;
    }

    Node* prev = head;          // Store old head

    head = head->next;          // Move head

    head->back = nullptr;       // New head has no previous node

    prev->next = nullptr;       // Disconnect old head

    delete prev;                // Free memory

    return head;                // Return new head
}
```

---

# 2. Delete Tail

## Code

```cpp
Node* deleteTail(Node* head){

    // Empty list
    if(head == nullptr){
        return nullptr;
    }

    // Only one node
    if(head->next == nullptr){
        delete head;
        return nullptr;
    }

    Node* tail = head;

    // Reach last node
    while(tail->next != nullptr){
        tail = tail->next;
    }

    Node* prev = tail->back;    // Previous node

    prev->next = nullptr;       // New tail

    tail->back = nullptr;       // Disconnect old tail

    delete tail;                // Delete old tail

    return head;
}
```

---

# 3. Delete Kth Node

> Assume **1-based indexing**

---

## Code

```cpp
Node* deleteK(Node* head, int k){

    // Empty list
    if(head == nullptr){
        return nullptr;
    }

    Node* temp = head;

    int cnt = 1;

    // Reach kth node
    while(temp != nullptr && cnt < k){
        temp = temp->next;
        cnt++;
    }

    // Invalid position
    if(temp == nullptr){
        return head;
    }

    // Delete head
    if(temp == head){
        return deleteHead(head);
    }

    Node* prev = temp->back;      // Previous node
    Node* front = temp->next;     // Next node

    // If deleting tail
    if(front == nullptr){

        prev->next = nullptr;

        temp->back = nullptr;

        delete temp;

        return head;
    }

    // Connect previous node to next node
    prev->next = front;

    // Connect next node back to previous node
    front->back = prev;

    // Disconnect current node
    temp->next = nullptr;
    temp->back = nullptr;

    delete temp;

    return head;
}
```

---

# 4. Delete Given Node

## Code

```cpp
void deleteNode(Node* node){

    Node* prev = node->back;      // Previous node

    Node* front = node->next;     // Next node

    // If node is the tail
    if(front == nullptr){

        prev->next = nullptr;

        node->back = nullptr;

        delete node;

        return;
    }

    prev->next = front;           // Previous points to next

    front->back = prev;           // Next points back to previous

    node->next = nullptr;         // Disconnect node

    node->back = nullptr;

    delete node;                  // Free memory
}
```

> **Note:** This function assumes the given node is **not the head**. If the head can also be passed, that case should be handled separately by calling `deleteHead()`.

---

# Pointer Update Order ⭐

Suppose

```
prev <-> curr <-> front
```

Delete

```
curr
```

Correct order

```cpp
prev->next = front;

front->back = prev;

delete curr;
```

Never delete the node **before** reconnecting its neighbors.

---

# Singly LL vs Doubly LL Deletion

| Singly LL | Doubly LL |
|------------|-----------|
| Need previous node | Previous node already available |
| More traversal | Easier pointer updates |
| next pointer only | next + back pointers |
| Harder deletion | Simpler deletion |

---

# Time Complexities

| Operation | Time |
|-----------|------|
| Delete Head | O(1) |
| Delete Tail | O(N) |
| Delete Kth Node | O(N) |
| Delete Given Node | O(1) |