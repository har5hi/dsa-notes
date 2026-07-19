# 160. Intersection of Two Linked Lists
---

# Problem Statement

Given the heads of two singly linked lists, return the node at which the two linked lists intersect.

If the two linked lists have no intersection, return `NULL`.

> **Note:** The intersection is based on **node references**, **not node values**.

---

## Example 

```
List A

4 -> 1
       \
        8 -> 4 -> 5

List B

5 -> 6 -> 1
           \
            8 -> 4 -> 5

Output

Node with value 8
```

---

# Important Note ⭐

Two linked lists intersect only if they share the **same node in memory**.

Wrong

```
List A

1 -> 2 -> 3

List B

4 -> 5 -> 3
```

Even though both have value `3`, these are different nodes.

Correct

```
      3 -> 4
     /
1 -> 2

5 -> 6
     \
      3 -> 4
```

Both lists point to the **same node**.

---

# Approach 1 : Brute Force (Nested Traversal)

## Intuition

For every node in List A,

traverse the entire List B.

If both pointers point to the same node,

return that node.

---

# Algorithm

1. Traverse every node of List A.
2. For each node,
   traverse List B.
3. Compare node addresses.
4. If same node is found,
   return it.
5. Otherwise return `NULL`.

---

# Code

```cpp
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {

        ListNode* a = headA;

        while(a){

            ListNode* b = headB;

            while(b){

                if(a == b)
                    return a;

                b = b->next;
            }

            a = a->next;
        }

        return nullptr;
    }
};
```

---

# Time Complexity

```
O(N × M)
```

---

# Space Complexity

```
O(1)
```

---

# Approach 2 : Better (Hash Set)

## Intuition

Store all nodes of List A in a hash set.

Traverse List B.

The first node already present in the hash set is the intersection node.

---

# Algorithm

1. Traverse List A.
2. Store every node address in a hash set.
3. Traverse List B.
4. If current node exists in the hash set,
   return it.
5. Otherwise return `NULL`.

---

# Code

```cpp
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {

        unordered_set<ListNode*> st;

        while(headA){
            st.insert(headA);
            headA = headA->next;
        }

        while(headB){

            if(st.count(headB))
                return headB;

            headB = headB->next;
        }

        return nullptr;
    }
};
```

---

# Time Complexity

```
O(N + M)
```

---

# Space Complexity

```
O(N)
```

---

# Approach 3 : Optimal (Two Pointer Switching)

## Intuition ⭐⭐⭐

Maintain two pointers.

```
Pointer A

Pointer B
```

Each pointer traverses both linked lists.

When a pointer reaches the end, move it to the head of the other list.

Eventually, both pointers travel the same total distance.

If an intersection exists, they meet there.

Otherwise, both become `NULL`.

---

# Why Does This Work?

Suppose

```
Length A = a + c

Length B = b + c
```

where

```
a = unique part of A

b = unique part of B

c = common part
```

Pointer A travels

```
a + c + b
```

Pointer B travels

```
b + c + a
```

Both travel the same distance.

Hence,

they meet at the intersection.

---

# Algorithm

1. Initialize pointers `a` and `b`.
2. Traverse both lists.
3. If pointer becomes `NULL`,
   move it to the other list.
4. Continue until both pointers become equal.
5. Return the meeting node.

---

# Code

```cpp
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {

        ListNode* a = headA;
        ListNode* b = headB;

        while(a != b){

            if(a == nullptr)
                a = headB;
            else
                a = a->next;

            if(b == nullptr)
                b = headA;
            else
                b = b->next;
        }

        return a;
    }
};
```

---

# Time Complexity

```
O(N + M)
```

Each pointer traverses each list at most once.

---

# Space Complexity

```
O(1)
```

---

# Comparison

| Approach | Time | Space | Idea |
|----------|------|-------|------|
| Nested Traversal | O(N × M) | O(1) | Compare every pair of nodes |
| Hash Set | O(N + M) | O(N) | Store node addresses |
| Two Pointer | O(N + M) | O(1) | Equalize path lengths by switching lists |

---