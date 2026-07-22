# 25. Reverse Nodes in k-Group

---

# Problem Statement

Given the head of a linked list, reverse the nodes of the list **k at a time**, and return the modified list.

- If the number of nodes is **less than k**, leave them as they are.
- You may **not** change the values inside the nodes.
- Only the node links should be changed.

---

# Key Observation ⭐

Instead of reversing the whole linked list,

we reverse **only k nodes at a time**.

If the remaining nodes are **less than k**,

they are left unchanged.

---

# Approach 1 : Brute Force (Using Extra Array)

## Intuition

Store the linked list values in a vector.

Reverse every block of size `k`.

Create the linked list again.

Although simple,

this violates the condition of **not changing node values**, so it is **not accepted on LeetCode**.

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

# Approach 2 : Optimal (Reverse Each Group)

## Intuition ⭐⭐⭐

For every group,

1. Check whether **k nodes exist**.
2. Reverse exactly **k nodes**.
3. Connect the reversed group with the remaining list.
4. Continue for the next group.

---

# Important Idea ⭐

Before reversing,

always check whether **k nodes are available**.

Suppose

```
1 -> 2 -> 3 -> 4 -> 5

k = 3
```

After reversing first group

```
3 -> 2 -> 1

Remaining

4 -> 5
```

Only **2 nodes remain**.

Since

```
2 < 3
```

they remain unchanged.

Final

```
3 -> 2 -> 1 -> 4 -> 5
```

---

# Algorithm

1. Count `k` nodes.
2. If fewer than `k`,
   return head.
3. Reverse first `k` nodes.
4. Recursively solve remaining list.
5. Connect last node of current group to the returned head.
6. Return new head.

---

# Code

```cpp
class Solution {
public:

    ListNode* reverseKGroup(ListNode* head, int k) {

        ListNode* temp = head;

        for(int i = 0; i < k; i++){
            if(temp == nullptr)
                return head;
            temp = temp->next;
        }

        ListNode* prev = nullptr;
        ListNode* curr = head;

        for(int i = 0; i < k; i++){

            ListNode* nextNode = curr->next;

            curr->next = prev;

            prev = curr;

            curr = nextNode;
        }

        head->next = reverseKGroup(curr, k);

        return prev;
    }
};
```

---

# Time Complexity

```
O(N)
```

Every node is visited once.

---

# Space Complexity

```
O(N / K)
```

Recursive call stack.

> If implemented iteratively, auxiliary space becomes **O(1)**.

---

# Why Count k Nodes First?

Suppose

```
1 -> 2 -> 3 -> 4 -> 5

k = 3
```

Without checking,

you might reverse

```
4 -> 5
```

which is **incorrect**.

Therefore,

always verify that

```
k nodes exist
```

before reversing.

---

# Comparison

| Approach | Time | Space | Idea |
|----------|------|-------|------|
| Extra Array | O(N) | O(N) | Store values and reverse groups (not accepted) |
| Recursive Pointer Reversal | O(N) | O(N/K) | Reverse nodes in groups of k |

---

 ListNode* curr = head;
        int count = 0;
        
        while (curr && count < k) {
            curr = curr->next;
            count++;
        }
        
        if (count == k) {
            curr = reverseKGroup(curr, k);
            
            while (count--) {
                ListNode* temp = head->next;
                head->next = curr;
                curr = head;
                head = temp;
            }
            head = curr;
        }
        
        return head;