# 138. Copy List with Random Pointer

---

# Problem Statement

A linked list is given where each node contains:

- `val`
- `next`
- `random`

The `random` pointer can point to **any node** in the list or can be **NULL**.

Create a **deep copy** of the linked list and return the head of the copied list.

> **Deep Copy** means creating completely new nodes. The copied list should not share any nodes with the original list.

---

## Node Structure

```cpp
class Node {
public:
    int val;
    Node* next;
    Node* random;

    Node(int _val) {
        val = _val;
        next = NULL;
        random = NULL;
    }
};
```

---

# Key Observation ⭐

We cannot simply copy the values because the **random pointers** must also point to the **new copied nodes**, not the original nodes.

---

# Approach 1 : Brute Force (HashMap)

## Intuition

Create a copy of every node first.

Store the mapping

```
Original Node

↓

Copied Node
```

Then use this mapping to correctly assign both `next` and `random` pointers.

---

# Algorithm

### First Traversal

Create a copy of every node.

Store

```
Original Node

↓

Copy Node
```

inside a HashMap.

---

### Second Traversal

Using the map,

assign

```
copy->next

copy->random
```

---

# Code

```cpp
class Solution {
public:

    Node* copyRandomList(Node* head) {

        if(head == nullptr)
            return nullptr;

        unordered_map<Node*, Node*> mp;

        Node* curr = head;

        while(curr){

            mp[curr] = new Node(curr->val);

            curr = curr->next;
        }

        curr = head;

        while(curr){

            mp[curr]->next = mp[curr->next];

            mp[curr]->random = mp[curr->random];

            curr = curr->next;
        }

        return mp[head];
    }
};
```

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

(HashMap)

---

# Approach 2 : Optimal (Node Interleaving)

## Intuition ⭐⭐⭐

Instead of using extra space,

insert every copied node **immediately after its original node**.

This allows us to access the copied node directly without using a HashMap.

---

# Algorithm

### Pass 1

Insert copied node after every original node.

---

### Pass 2

Assign random pointer

```
copy->random

=

original->random->next
```

---

### Pass 3

Separate original and copied lists.

---

# Code

```cpp
class Solution {
public:

    Node* copyRandomList(Node* head) {

        if(head == nullptr)
            return nullptr;

        Node* curr = head;

        // Step 1: Insert copied nodes
        while(curr){

            Node* copy = new Node(curr->val);

            copy->next = curr->next;

            curr->next = copy;

            curr = copy->next;
        }

        // Step 2: Copy random pointers
        curr = head;

        while(curr){

            if(curr->random)
                curr->next->random = curr->random->next;

            curr = curr->next->next;
        }

        // Step 3: Separate both lists
        curr = head;

        Node* dummy = new Node(0);

        Node* copyTail = dummy;

        while(curr){

            Node* copy = curr->next;

            curr->next = copy->next;

            copyTail->next = copy;

            copyTail = copy;

            curr = curr->next;
        }

        return dummy->next;
    }
};
```

---

# Time Complexity

```
O(N)
```

Three linear traversals.

---

# Space Complexity

```
O(1)
```

(No extra HashMap)

---

# Comparison

| Approach | Time | Space | Idea |
|----------|------|-------|------|
| HashMap | O(N) | O(N) | Store mapping from original node to copied node |
| Node Interleaving | O(N) | O(1) | Insert copied nodes between original nodes |

---

# Why Does `curr->random->next` Work? ⭐

Suppose

```
Original

1 ----> 2 ----> 3

1.random = 3
```

After inserting copies

```
1 -> 1' -> 2 -> 2' -> 3 -> 3'
```

Notice

```
Original Node

↓

Copy Node
```

Every copied node is **immediately after** its original node.

So

```
3.next

↓

3'
```

Therefore

```
1'.random

=

1.random->next

=

3'
```

This trick removes the need for a HashMap.

---

# Interview Tips ⭐

This problem is a classic example of **pointer manipulation**.

Remember the three-pass pattern:

```
Insert Copy Nodes
        │
        ▼
Assign Random Pointers
        │
        ▼
Separate the Two Lists
```

The key trick is realizing that every copied node sits immediately after its original node, allowing direct access without extra space.

---