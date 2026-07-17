# 328. Odd Even Linked List

---

# Problem Statement

Given the `head` of a singly linked list, group all the nodes with **odd indices** together followed by the nodes with **even indices**.
Return the reordered linked list.

> **Note:** Odd and Even refer to the **node positions (indices)**, **not** the node values.

The relative order within the odd and even groups should remain the same.

---

# Important Note ⭐

This problem is based on **node positions**, not values.

```
1 -> 4 -> 3 -> 2
```

Positions

```
1st → Odd
2nd → Even
3rd → Odd
4th → Even
```

Output

```
1 -> 3 -> 4 -> 2
```

NOT

```
1 -> 3 -> 2 -> 4
```

---

# Approach 1 : Brute Force (Using Two Separate Lists)

## Intuition

Create two separate linked lists.

- One stores nodes at **odd positions**
- One stores nodes at **even positions**

After traversing the entire list,

connect the odd list with the even list.

---

# Algorithm

1. Create two dummy nodes.
2. Traverse the linked list.
3. If position is odd,
   attach the node to the odd list.
4. Otherwise,
   attach it to the even list.
5. Connect the odd list with the even list.
6. Make the last even node point to `NULL`.
7. Return the odd list.

---

# Code

```cpp
class Solution {
public:
    ListNode* oddEvenList(ListNode* head) {

        if(head == nullptr){
            return head;
        }

        ListNode oddDummy(0);
        ListNode evenDummy(0);

        ListNode* oddTail = &oddDummy;
        ListNode* evenTail = &evenDummy;

        ListNode* curr = head;
        int pos = 1;

        while(curr){

            if(pos % 2 == 1){

                oddTail->next = curr;
                oddTail = oddTail->next;
            }
            else{

                evenTail->next = curr;
                evenTail = evenTail->next;
            }

            curr = curr->next;
            pos++;
        }

        oddTail->next = evenDummy.next;
        evenTail->next = nullptr;

        return oddDummy.next;
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
O(1)
```

Only two dummy nodes are used.

---

# Approach 2 : Optimal (In-Place Rearrangement)

## Intuition ⭐⭐⭐

Instead of creating two separate lists, rearrange the existing pointers.

Maintain

```
Odd Pointer
Even Pointer
```

Odd pointer always points to the next odd-positioned node.
Even pointer always points to the next even-positioned node.
Finally, attach the even list after the odd list.

---

# Algorithm

1. If the list has less than two nodes, return it.
2. Initialize
   - `odd = head`
   - `even = head->next`
3. Store `evenHead`.
4. Move odd pointer to the next odd node.
5. Move even pointer to the next even node.
6. Continue until the even list ends.
7. Attach the even list after the odd list.
8. Return head.

---

# Code

```cpp
class Solution {
public:
    ListNode* oddEvenList(ListNode* head) {

        if(head == nullptr || head->next == nullptr){
            return head;
        }

        ListNode* odd = head;
        ListNode* even = head->next;

        ListNode* evenHead = even;

        while(even != nullptr && even->next != nullptr){

            odd->next = even->next;
            odd = odd->next;

            even->next = odd->next;
            even = even->next;
        }

        odd->next = evenHead;

        return head;
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
O(1)
```

---

# Comparison

| Approach | Time | Space | Idea |
|----------|------|-------|------|
| Brute Force | O(N) | O(1) | Create separate odd & even lists using dummy nodes |
| Optimal | O(N) | O(1) | Rearrange the original list in-place |

---