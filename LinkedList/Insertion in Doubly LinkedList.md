# Insertion in Doubly Linked List (C++) 🚀

> A **Doubly Linked List (DLL)** is a linked list in which every node has **two pointers**:
>
> - `next` → points to the next node
> - `back` → points to the previous node
>
> Because every node knows both its next and previous node, insertion and deletion become easier compared to a Singly Linked List.

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

# Pointer Rules ⭐

Whenever inserting a node between two nodes,

you must update **four pointers**.

```
prev <-> newNode <-> next
```

Order matters!

---

# 1. Insert Before Head

## Code

```cpp
Node* insertBeforeHead(Node* head, int val){

    Node* newHead = new Node(val, head, nullptr);   // Create new node

    head->back = newHead;                           // Old head points back to new node

    return newHead;                                 // Return new head
}
```

---

# 2. Insert Before Tail

## Code

```cpp
Node* insertBeforeTail(Node* head, int val){

    // Empty or single-node list
    if(head == nullptr || head->next == nullptr){
        return insertBeforeHead(head, val);
    }

    Node* tail = head;

    // Reach last node
    while(tail->next != nullptr){
        tail = tail->next;
    }

    Node* prev = tail->back;      // Node before tail

    Node* newNode = new Node(val, tail, prev);  // Create new node

    prev->next = newNode;         // Previous node points to new node

    tail->back = newNode;         // Tail points back to new node

    return head;
}
```

---

# 3. Insert Before Kth Node

## Code

```cpp
Node* insertBeforeK(Node* head, int k, int val){

    // Insert before first node
    if(k == 1){
        return insertBeforeHead(head, val);
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

    Node* prev = temp->back;     // Previous node

    Node* newNode = new Node(val, temp, prev); // New node

    prev->next = newNode;        // Previous node points to new node

    temp->back = newNode;        // Current node points back

    return head;
}
```

---

# 4. Insert Before Given Node

## Code

```cpp
void insertBeforeNode(Node* node, int val){

    Node* prev = node->back;         // Previous node
    Node* newNode = new Node(val, node, prev);   // New node
    prev->next = newNode;            // Previous points to new node
    node->back = newNode;            // Current points back
}
```

---

# Pointer Update Order ⭐

Suppose

```
prev <-> curr
```

Need

```
prev <-> newNode <-> curr
```

Correct order

```cpp
Node* newNode = new Node(val, curr, prev);

prev->next = newNode;

curr->back = newNode;
```

---

# Singly LL vs Doubly LL

| Singly LL | Doubly LL |
|------------|-----------|
| next only | next + back |
| Cannot move backward | Can move both directions |
| Less memory | More memory |
| Easier implementation | Easier insertion/deletion |

---

# Time Complexities

| Operation | Time |
|-----------|------|
| Insert Before Head | O(1) |
| Insert Before Tail | O(N) |
| Insert Before Kth Node | O(N) |
| Insert Before Given Node | O(1) |

---

# Interview Tips ⭐

- A Doubly Linked List requires updating **both `next` and `back` pointers**.
- Before inserting between two nodes, identify:
  - `prev`
  - `curr`
- Draw the links on paper if you're confused.
- Handle these edge cases:
  - Empty list
  - Single-node list
  - Insert before head
  - Invalid position
- If you're given the actual node (not the position), insertion can often be done in **O(1)**.