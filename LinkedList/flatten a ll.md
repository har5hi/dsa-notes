# Flattening a Linked List (Singly & Doubly)

---

# What is Flattening?

Flattening means converting a **multi-level linked list** into a **single linear linked list**.

There are two common interview problems:

1. **Flatten a Multilevel Doubly Linked List** (LeetCode 430)
2. **Flattening a Linked List** (GFG - Bottom Pointer Linked List)

Although both are called *Flatten Linked List*, their structures are completely different.

---

# Types of Flattening

| Problem | Link Used | Final Structure |
|---------|-----------|----------------|
| GFG Flatten Linked List | `next` + `bottom` | Single Bottom Linked List |
| LeetCode 430 | `next` + `child` + `prev` | Normal Doubly Linked List |

---

# 1. Flattening a Linked List (GFG)

## Structure

Each node has

```cpp
struct Node{
    int data;
    Node* next;
    Node* bottom;
};
```

---

Example

```
5 ----> 10 ----> 19 ----> 28
|        |         |        |
7        20        22       35
|                  |        |
8                  50       40
|                           |
30                          45
```

Each vertical list is already **sorted**.

Goal

```
5
↓

7
↓

8
↓

10
↓

19
↓

20
↓

22
↓

28
↓

30
↓

35
↓

40
↓

45
↓

50
```

using only

```
bottom
```

pointers.

---

# Key Observation ⭐

Each vertical list is already sorted.
This is exactly like merging **K Sorted Linked Lists**.

---

# Approach 1 : Brute Force

## Intuition

Store every value in an array.
Sort it.
Create a new linked list.

---

# Time Complexity

```
O(N log N)
```

---

# Space Complexity

```
O(N)
```

---

# Approach 2 : Optimal (Recursive Merge)

## Intuition ⭐⭐⭐

Flatten the list from right to left.

```
Flatten(next)

↓

Merge

↓

Return Head
```

Exactly like Merge Sort.

---

# Algorithm

1. Flatten the right list.
2. Merge current list with flattened list.
3. Return merged list.

---

# Code

```cpp
Node* merge(Node* a, Node* b){

    if(a == nullptr) return b;
    if(b == nullptr) return a;

    Node* result;

    if(a->data < b->data){

        result = a;
        result->bottom = merge(a->bottom, b);
    }
    else{

        result = b;
        result->bottom = merge(a, b->bottom);
    }

    result->next = nullptr;

    return result;
}

Node* flatten(Node* root){

    if(root == nullptr || root->next == nullptr)
        return root;

    root->next = flatten(root->next);

    root = merge(root, root->next);

    return root;
}
```

---

# Time Complexity

```
O(N × M)
```

where merging happens recursively.
(Overall complexity is often written as **O(NK)** depending on notation.)

---

# Space Complexity

```
O(N)
```

Recursive stack.

---

---

# 2. Flatten a Multilevel Doubly Linked List (LeetCode 430)

## Structure

```cpp
class Node{

public:

    int val;

    Node* prev;

    Node* next;

    Node* child;
};
```

---

# Key Observation ⭐

Whenever we find a

```
child
```

we should

1. Flatten the child list.
2. Insert it between current and next.

---

# Approach 1 : Brute Force

Use

```
Stack
```

Store remaining nodes.
Whenever a child appears,
visit it first.

---

# Time Complexity

```
O(N)
```

---

# Space Complexity

```
O(N)
```

---

# Approach 2 : Optimal (DFS Recursion)

## Intuition ⭐⭐⭐

Depth First Traversal

```
Current

↓

Child

↓

Next
```

Flatten the child first.

Then reconnect the next part.

---

# Algorithm

For every node

If child exists

```
Flatten Child

↓

Save Next

↓

Connect Child

↓

Find Child Tail

↓

Connect Old Next
```

Repeat.

---

# Code

```cpp
class Solution {
public:

    Node* flatten(Node* head) {

        if(head == nullptr)
            return head;

        Node* curr = head;

        while(curr){

            if(curr->child){

                Node* nextNode = curr->next;

                Node* childHead = flatten(curr->child);

                curr->next = childHead;
                childHead->prev = curr;

                curr->child = nullptr;

                Node* tail = childHead;

                while(tail->next){
                    tail = tail->next;
                }

                tail->next = nextNode;

                if(nextNode)
                    nextNode->prev = tail;
            }

            curr = curr->next;
        }

        return head;
    }
};
```

---

# Time Complexity

```
O(N²)
```

because we repeatedly search for the child tail.

---

# Comparison

| Problem | Extra Pointer | Technique | Time |
|---------|---------------|-----------|------|
| GFG Flatten | bottom | Merge Sorted Lists | O(NK) / depends on notation |
| LC 430 | child | DFS | O(N) (optimized) |

---