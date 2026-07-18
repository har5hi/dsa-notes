# 19. Remove Nth Node From End of List

---

# Problem Statement

Given the `head` of a linked list, remove the **nth node from the end** of the list and return its head.

---

# Approach 1 : Brute Force (Find Length)

## Intuition

First find the **length** of the linked list.

Suppose

```
Length = L
```

The node to remove is

```
(L - n + 1)th node from the beginning.
```

To delete it, move to the **previous node**

```
(L - n)th node
```

and change its `next` pointer.

---

# Algorithm

1. Find the length of the linked list.
2. If `length == n`, delete the head node.
3. Otherwise move to the `(length-n)`th node.
4. Skip the next node.
5. Return the head.

---

# Code

```cpp
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {

        int len = 0;

        ListNode* temp = head;

        while(temp){
            len++;
            temp = temp->next;
        }

        // Remove head
        if(len == n){

            ListNode* node = head;
            head = head->next;
            delete node;

            return head;
        }

        temp = head;

        for(int i = 1; i < len - n; i++){
            temp = temp->next;
        }

        ListNode* node = temp->next;

        temp->next = temp->next->next;

        delete node;

        return head;
    }
};
```

---

# Time Complexity

```
O(N) + O(N)

≈ O(N)
```

---

# Space Complexity

```
O(1)
```

---

# Approach 2 : Optimal (Two Pointer)

## Intuition ⭐⭐⭐

Maintain two pointers

```
Fast

Slow
```

Move the **Fast** pointer `n` steps ahead. Now move both pointers one step at a time.
When Fast reaches the last node, Slow will be just before the node that needs to be deleted.

---

# Why Dummy Node?

A dummy node helps handle cases where the **head itself has to be deleted**.
Without a dummy node, extra conditions are needed.
With a dummy node, the code becomes simpler and handles all cases uniformly.

---

# Algorithm

1. Create a dummy node.
2. Point dummy to head.
3. Move Fast pointer `n` steps.
4. Move Fast and Slow together until Fast reaches the last node.
5. Delete `slow->next`.
6. Return `dummy.next`.

---

# Code

```cpp
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {

        ListNode dummy(0);
        dummy.next = head;

        ListNode* fast = &dummy;
        ListNode* slow = &dummy;

        // Move fast n steps ahead
        for(int i = 0; i < n; i++){
            fast = fast->next;
        }

        // Move both pointers
        while(fast->next){

            fast = fast->next;
            slow = slow->next;
        }

        ListNode* node = slow->next;

        slow->next = slow->next->next;

        delete node;

        return dummy.next;
    }
};
```

---

# Time Complexity

```
O(N)
```

Only one traversal is required.

---

# Space Complexity

```
O(1)
```

---

# Comparison

| Approach | Time | Space |
|----------|------|-------|
| Find Length | O(N) | O(1) |
| Two Pointer | O(N) | O(1) |

Although both have the same time complexity, the optimal solution performs the task in **one traversal**, making it the preferred interview approach.

---