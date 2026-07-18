# 876. Middle of the Linked List

---

# Problem Statement

Given the `head` of a singly linked list, return the **middle node** of the linked list.
- If there are **two middle nodes**, return the **second middle node**.

---

## Example 1

```
Input:

1 -> 2 -> 3 -> 4 -> 5

Output:

3 -> 4 -> 5
```

---

## Example 2

```
Input:

1 -> 2 -> 3 -> 4 -> 5 -> 6

Output:

4 -> 5 -> 6
```
Since there are two middle nodes (3 and 4), return the **second middle node (4).**

---

# Approach 1: Brute Force (Find Length)

## Idea

1. Traverse the linked list and calculate its length.
2. Compute the middle index.
3. Traverse again until the middle node.
4. Return it.

---

## Code

```cpp
class Solution {
public:
    ListNode* middleNode(ListNode* head) {

        int cnt = 0;

        ListNode* temp = head;

        // Count nodes
        while(temp){
            cnt++;
            temp = temp->next;
        }

        int mid = cnt / 2;
        temp = head;

        // Reach middle node
        while(mid--){
            temp = temp->next;
        }
        return temp;
    }
};
```

---

# Approach 2: Optimal (Slow & Fast Pointer)

## Intuition ⭐

Instead of traversing twice,

Use **two pointers**.

- Slow moves **1 step**
- Fast moves **2 steps**

When the fast pointer reaches the end,
the slow pointer automatically reaches the middle.

---

# Why Does This Work?

Suppose

```
Fast moves

2 nodes
```

while

```
Slow moves

1 node
```

When Fast finishes travelling

```
N nodes
```

Slow has travelled

```
N / 2
```

which is exactly the middle.

---

# Code

```cpp
class Solution {
public:
    ListNode* middleNode(ListNode* head) {

        ListNode* slow = head;
        ListNode* fast = head;

        while(fast != nullptr && fast->next != nullptr){
            slow = slow->next;
            fast = fast->next->next;
        }
        return slow;
    }
};
```

---

# Why is This Better?

| Brute | Optimal |
|--------|----------|
| Two Traversals | One Traversal |
| O(N) | O(N) |
| O(1) | O(1) |
| Count Nodes | Two Pointer Technique |

Although both have the same Big-O complexity, the optimal solution traverses the list only once and is the preferred interview approach.

---

# Interview Tip ⭐

Whenever you see questions involving:

- Middle of Linked List
- Cycle Detection
- Palindrome Linked List
- Find Nth Node from End
- Happy Number

Think of the **Slow & Fast Pointer (Tortoise and Hare)** technique.

---

# 2095. Delete the Middle Node of a Linked List

---

# Problem Statement

Given the `head` of a singly linked list, delete the **middle node** and return the head of the modified linked list.

- If the list has **only one node**, return `NULL`.
- The middle node is the **⌊n / 2⌋th** node (0-indexed).

---

# Approach 1 : Brute Force (Find Length)

## Intuition

First find the length of the linked list.

The middle node is at

```
Length / 2
```

Move to the node just before the middle node and delete it.

---

# Code

```cpp
class Solution {
public:
    ListNode* deleteMiddle(ListNode* head) {

        if(head == nullptr || head->next == nullptr){
            return nullptr;
        }

        int len = 0;

        ListNode* temp = head;

        while(temp){
            len++;
            temp = temp->next;
        }

        int middle = len / 2;

        temp = head;

        for(int i = 1; i < middle; i++){
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

# Approach 2 : Optimal (Slow & Fast Pointer)

## Intuition ⭐⭐⭐

Use Slow and Fast pointers.

- Slow moves **one step**.
- Fast moves **two steps**.

Maintain one extra pointer

```
prev
```

which always points to the node before `slow`.

When `fast` reaches the end,

`slow` points to the middle node.

`prev` points to the node before the middle.

Delete

```
slow
```

by changing

```
prev->next
```

---

# Why Do We Need `prev`?

The slow pointer points to the middle node.

To delete it, we need the previous node.

```
Prev -> Slow -> Next
```

Change

```
Prev->next = Slow->next
```

Now the middle node is removed.

---

# Code

```cpp
class Solution {
public:
    ListNode* deleteMiddle(ListNode* head) {

        if(head == nullptr || head->next == nullptr){
            return nullptr;
        }

        ListNode* slow = head;
        ListNode* fast = head;
        ListNode* prev = nullptr;

        while(fast != nullptr && fast->next != nullptr){

            prev = slow;

            slow = slow->next;

            fast = fast->next->next;
        }

        prev->next = slow->next;

        delete slow;

        return head;
    }
};
```

```cpp
class Solution {
public:
    ListNode* deleteMiddle(ListNode* head) {

        if(head == nullptr || head->next == nullptr)
            return nullptr;

        ListNode* slow = head;
        ListNode* fast = head->next->next;

        while(fast != nullptr && fast->next != nullptr){
            slow = slow->next;
            fast = fast->next->next;
        }

        slow->next = slow->next->next;

        return head;
    }
};
```

---

# Time Complexity

```
O(N)
```

Only one traversal.

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
| Slow & Fast Pointer | O(N) | O(1) |

The optimal approach performs the deletion in **one traversal**, making it the preferred interview solution.

---