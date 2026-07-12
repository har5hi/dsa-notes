# Deletion in Singly Linked List (C++) 🗑️

> **Important:** We never shift nodes like in arrays. We only reconnect the `next` pointers and free the deleted node's memory.

---

# Initial Linked List

Suppose we have

```
head
 |
 v
10 -> 20 -> 30 -> 40 -> 50 -> NULL
```

---

# 1. Delete Head

## Idea

The head node is the first node.
Simply move the head to the second node and delete the old head.

---

## Code

```cpp
Node* deleteHead(Node* head){

    // Empty linked list
    if(head == nullptr){
        return nullptr;
    }

    Node* temp = head;          // Store old head
    head = head->next;          // Move head to second node
    delete temp;                // Free memory of old head

    return head;                // Return new head
}
```

---

# 2. Delete Tail

## Idea

Reach the second last node.

Delete the last node.

Make second last node point to `nullptr`.

---

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

    Node* temp = head;      // Used for traversal

    // Reach second last node
    while(temp->next->next != nullptr){
        temp = temp->next;
    }

    delete temp->next;      // Delete last node
    temp->next = nullptr;   // New last node
    return head;
}
```

---

# 3. Delete Kth Node

> Assume **1-based indexing**.

## Idea

Reach the (k-1)th node.

Store kth node.

Reconnect pointers.

Delete kth node.

---

## Code

```cpp
Node* deleteK(Node* head, int k){

    // Empty list
    if(head == nullptr){
        return nullptr;
    }

    // Delete first node
    if(k == 1){
        return deleteHead(head);
    }

    Node* temp = head;      // Traversal pointer

    int cnt = 1;            // Current position

    // Reach (k-1)th node
    while(temp != nullptr && cnt < k-1){
        temp = temp->next;
        cnt++;
    }

    // Invalid position
    if(temp == nullptr || temp->next == nullptr){
        return head;
    }

    Node* nodeToDelete = temp->next;     // Store kth node
    temp->next = nodeToDelete->next;     // Skip kth node
    delete nodeToDelete;                 // Delete kth node

    return head;
}
```

---

# 4. Delete Node with Value X

## Idea

Traverse until

```
temp->next->data == x
```

Skip that node.

Delete it.

---

## Code

```cpp
Node* deleteValue(Node* head, int x){

    // Empty linked list
    if(head == nullptr){
        return nullptr;
    }

    // Value is at head
    if(head->data == x){
        return deleteHead(head);
    }

    Node* temp = head;      // Traversal pointer

    // Reach node before x
    while(temp->next != nullptr && temp->next->data != x){
        temp = temp->next;
    }

    // Value not found
    if(temp->next == nullptr){
        return head;
    }

    Node* nodeToDelete = temp->next;     // Store node
    temp->next = nodeToDelete->next;     // Skip node
    delete nodeToDelete;                 // Free memory

    return head;
}
```

---

# Why This Order?

Correct

```cpp
Node* nodeToDelete = temp->next;
temp->next = nodeToDelete->next;
delete nodeToDelete;
```

Why?

Because after deleting the node, you **cannot safely access it anymore**. Always save the pointer and reconnect the list **before** deleting.

---

# Difference Between Insertion & Deletion

### Insertion

```
prev -> next
```

becomes

```
prev -> newNode -> next
```

---

### Deletion

```
prev -> deleteNode -> next
```

becomes

```
prev -------------> next
```

The deleted node is then freed using

```cpp
delete nodeToDelete;
```

---

# Summary

| Operation | Time | Space |
|-----------|------|-------|
| Delete Head | O(1) | O(1) |
| Delete Tail | O(N) | O(1) |
| Delete Kth Node | O(N) | O(1) |
| Delete by Value | O(N) | O(1) |

---

# Interview Tips ⭐

- Deleting the **head** is the easiest case.
- For deleting any other node, first find the **previous node**.
- Never lose access to the node you want to delete—store it in a temporary pointer.
- Always reconnect the list **before** deleting the node.
- Handle these edge cases:
  - Empty list
  - Single-node list
  - Delete first node
  - Delete last node
  - Invalid position
  - Value not found
- If the first node changes, always return the **new head**.

---