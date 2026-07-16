# Reversing a Linked List

---

# Why Reverse?

Suppose we have

```
head
 |
 v
10 -> 20 -> 30 -> 40 -> NULL
```

After reversing

```
head
 |
 v
40 -> 30 -> 20 -> 10 -> NULL
```

Notice

- Head becomes Tail
- Tail becomes Head
- Every arrow changes direction

---

# Reverse a Singly Linked List

## Idea

Each node only knows its **next** node.

If we directly reverse

```
curr->next = prev;
```

we'll lose the remaining list.
So before changing any pointer, we first save

```
next = curr->next;
```

This is the most important concept.

---

# Three Pointer Technique ⭐⭐⭐

```
prev

curr

next
```

Initially

```
prev = NULL

curr = head

next = NULL
```

---

## Code (Iterative)

```cpp
Node* reverseLL(Node* head){

    Node* prev = nullptr;          // Previous node

    Node* curr = head;             // Current node

    while(curr != nullptr){

        Node* next = curr->next;   // Save next node before changing pointers
        curr->next = prev;         // Reverse the link
        prev = curr;               // Move prev one step ahead
        curr = next;               // Move curr one step ahead
    }

    return prev;                   // Prev becomes the new head
}
```

---

# Why Do We Need "next"?

Suppose we do

```cpp
curr->next = prev;
```

without saving

```
curr->next
```

Example

```
10 -> 20 -> 30
```

After

```
10 -> NULL
```

Where did

```
20 -> 30
```

go?

Lost forever.

Hence

```cpp
Node* next = curr->next;
```

is mandatory.

---

# Recursive Solution

## Idea

Go till the last node.
While returning,
reverse every pointer.

---

## Code

```cpp
Node* reverseLL(Node* head){

    // Empty list or single node
    if(head == nullptr || head->next == nullptr){
        return head;
    }

    Node* newHead = reverseLL(head->next);   // Reverse remaining list
    head->next->next = head;                 // Reverse current link
    head->next = nullptr;                    // Make current node last

    return newHead;                          // Return new head
}
```

---

# Summary

| Method | Time | Space |
|---------|------|-------|
| Iterative | O(N) | O(1) |
| Recursive | O(N) | O(N) (Recursion Stack) |

---

# Reverse a Doubly Linked List

Since every node has

```
next

back
```

we simply swap them.

---

# Key Idea ⭐

For every node

Swap

```
next

back
```

Move using the **old next** (stored after swap as `back`).

---

# Code

```cpp
Node* reverseDLL(Node* head){

    // Empty list or single node
    if(head == nullptr || head->next == nullptr){
        return head;
    }

    Node* curr = head;             // Start from head
    Node* last = nullptr;          // Will store the new head

    while(curr != nullptr){
        // Swap next and back pointers
        Node* temp = curr->next;
        curr->next = curr->back;
        curr->back = temp;

        last = curr;               // Update last processed node

        curr = temp;               // Move to the original next node
    }
    return last;                   // Last processed node becomes new head
}
```

---

# Singly vs Doubly Reversal

| Singly LL | Doubly LL |
|------------|-----------|
| Need 3 pointers (`prev`, `curr`, `next`) | Simply swap `next` and `back` |
| Reverse only `next` | Swap both pointers |
| Return `prev` | Return `last` |
| O(N) | O(N) |
| O(1) Space | O(1) Space |

---

# Pointer Cheat Sheet ⭐

## Singly Linked List

```
next = curr->next
curr->next = prev
prev = curr
curr = next
```

---

## Doubly Linked List

```
temp = curr->next
curr->next = curr->back
curr->back = temp
last = curr
curr = temp
```

---

# Interview Tips ⭐

- **SLL:** Never forget to save `next` before reversing the pointer.
- **DLL:** Reversal is just swapping `next` and `back` for every node.
- Always draw the linked list before coding.
- Check edge cases:
  - Empty list
  - Single-node list
---

# Practice Questions

- LeetCode 206 - Reverse Linked List ⭐⭐⭐
- LeetCode 92 - Reverse Linked List II ⭐⭐⭐
- LeetCode 25 - Reverse Nodes in k-Group ⭐⭐⭐⭐
- LeetCode 234 - Palindrome Linked List ⭐⭐⭐
- LeetCode 143 - Reorder List ⭐⭐⭐