# Insertion in Singly Linked List

---

# Node Structure

```cpp
class Node {
public:
    int data;
    Node* next;

    Node(int data1) {
        data = data1;
        next = nullptr;
    }
};
```

---

# Before Learning Insertion...

Suppose we have the following linked list:

```
head
 |
 v
10 -> 20 -> 30 -> 40 -> NULL
```

Whenever we insert a new node, **only the pointers (`next`) change**.

Remember:
- We never move the actual nodes.
- We only connect and reconnect pointers.

---

# 1. Insert at Head

## Idea

Create a new node.
Make its `next` point to the current head.
Return the new node as the new head.

---

## Code

```cpp
Node* insertHead(Node* head, int val){

    Node* temp = new Node(val);     // Create a new node with the given value
    temp->next = head;              // Connect new node to the current head

    return temp;                    // New node becomes the new head
}
```

---

# 2. Insert at Tail

## Idea

Traverse till the last node.
Create a new node.
Attach it after the last node.

---

## Code

```cpp
Node* insertTail(Node* head, int val){

    // If the linked list is empty
    if(head == nullptr){
        return new Node(val);           // New node becomes the head
    }

    Node* temp = head;                  // Used for traversal

    // Reach the last node
    while(temp->next != nullptr){
        temp = temp->next;
    }

    temp->next = new Node(val);         // Attach new node at the end

    return head;                        // Head remains unchanged
}
```

---

# 3. Insert at Kth Position

> Here, **k is 1-based indexing.**

---

## Idea

Reach the node just before the kth position.
Create a new node.
Adjust pointers.

---

## Code

```cpp
Node* insertK(Node* head, int val, int k){

    // Invalid position
    if(k <= 0){
        return head;
    }

    // Insert at first position
    if(k == 1){
        return insertHead(head, val);
    }

    Node* temp = head;              // Used for traversal

    int cnt = 1;                    // Current node position

    // Reach the (k-1)th node
    while(temp != nullptr && cnt < k-1){
        temp = temp->next;
        cnt++;
    }

    // Position doesn't exist
    if(temp == nullptr){
        return head;
    }

    Node* newNode = new Node(val);      // Create new node
    newNode->next = temp->next;         // New node points to next node
    temp->next = newNode;               // Previous node points to new node

    return head;
}
```

---

# 4. Insert Before a Given Value X

## Idea

Traverse until

```
temp->next->data == x
```

Create a node. Insert it between `temp` and `temp->next`.

---

## Code

```cpp
Node* insertBeforeValue(Node* head, int val, int x){

    // Empty linked list
    if(head == nullptr){
        return nullptr;
    }

    // Value is at the first node
    if(head->data == x){
        return insertHead(head, val);
    }

    Node* temp = head;                  // Used for traversal

    // Find the node before x
    while(temp->next != nullptr && temp->next->data != x){
        temp = temp->next;
    }

    // x not found
    if(temp->next == nullptr){
        return head;
    }

    Node* newNode = new Node(val);      // Create new node
    newNode->next = temp->next;         // New node points to x
    temp->next = newNode;               // Previous node points to new node

    return head;
}
```

---

# Why This Order?

Always remember this sequence:
```cpp
newNode->next = temp->next;
temp->next = newNode;
```
Never reverse them.
Wrong

```cpp
temp->next = newNode;
newNode->next = temp->next;
```

This creates
```
newNode
↓
newNode
```
and the remaining list gets disconnected.

---

# Summary

| Operation | Time | Space |
|-----------|------|-------|
| Insert at Head | O(1) | O(1) |
| Insert at Tail | O(N) | O(1) |
| Insert at Kth Position | O(N) | O(1) |
| Insert Before Value X | O(N) | O(1) |

---

# Interview Tips ⭐

- Insertion at the head is the easiest—master it first.
- Whenever inserting **between two nodes**, update pointers in this order:
  ```cpp
  newNode->next = prev->next;
  prev->next = newNode;
  ```
- Draw the linked list on paper before coding.
- Always think about edge cases:
  - Empty list
  - Single node
  - Inserting at head
  - Inserting at tail
  - Invalid position
  - Value not found
- If your code modifies the first node, the function should usually **return the new head**.

---