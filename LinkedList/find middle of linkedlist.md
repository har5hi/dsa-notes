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