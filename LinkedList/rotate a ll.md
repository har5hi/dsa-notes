# 61. Rotate List

---

# Problem Statement

Given the head of a linked list, rotate the list to the right by `k` places.
Return the head of the rotated linked list.

---

## Example 1

```
Input

Head

1 -> 2 -> 3 -> 4 -> 5

k = 2

Output

4 -> 5 -> 1 -> 2 -> 3
```

---

# Key Observation ⭐

Rotating one time means

```
Last Node

↓

Becomes

↓

New Head
```

Instead of rotating one step repeatedly,

we can find the **new tail** directly.

---

# Approach 1 : Brute Force

## Intuition

Rotate the linked list **one position** exactly `k` times.

Each rotation

- Find the last node.
- Make it the new head.
- Connect it with the old head.

---

# Algorithm

1. Repeat `k` times:
   - Find the last node.
   - Find the second last node.
   - Break the link.
   - Move last node to the front.
2. Return head.

---

# Time Complexity

```
O(N × K)
```

---

# Space Complexity

```
O(1)
```

---

# Approach 2 : Optimal (Circular Linked List)

## Intuition ⭐⭐⭐

Instead of rotating repeatedly,

convert the linked list into a **circular linked list**.

Then,

find the new tail.

Break the circle.

Done.

---

# Why k % Length ?

Suppose

```
1 -> 2 -> 3 -> 4 -> 5

Length = 5
```

Rotate

```
k = 5
```

Result

```
Same List
```

Rotate

```
k = 10
```

Again

```
Same List
```

Therefore

```
Effective Rotation

k = k % Length
```

---

# Algorithm

1. Find the length of the list.
2. Find the tail.
3. Connect tail with head (make circular).
4. Compute

```
k = k % length
```

5. Find the new tail

```
length - k - 1
```

6. New head is

```
newTail->next
```

7. Break the circle.
8. Return new head.

---

# Code

```cpp
class Solution {
public:
    ListNode* rotateRight(ListNode* head, int k) {

        if(head == nullptr || head->next == nullptr || k == 0)
            return head;

        int length = 1;

        ListNode* tail = head;

        while(tail->next){

            tail = tail->next;
            length++;
        }

        k = k % length;

        if(k == 0)
            return head;

        tail->next = head;

        int steps = length - k - 1;

        ListNode* newTail = head;

        while(steps--){

            newTail = newTail->next;
        }

        ListNode* newHead = newTail->next;

        newTail->next = nullptr;

        return newHead;
    }
};
```

---

# Time Complexity

```
O(N)
```

One traversal for finding length.

One traversal to find the new tail.

Overall,

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
| Rotate One by One | O(N × K) | O(1) | Perform one rotation repeatedly |
| Circular Linked List | O(N) | O(1) | Make the list circular and break at the correct position |

---

# Why Make It Circular?

Instead of moving the last node repeatedly,

we temporarily connect

```
Tail

↓

Head
```

forming a cycle.

Then,

we simply break the cycle at the correct position.

This makes the solution linear.

---

# Interview Tips ⭐

Whenever a linked list involves

- rotating,
- shifting,
- moving the tail to the front,

think about **making the list circular**.

Pattern:

```
Find Length

↓

Find Tail

↓

Make Circular

↓

Find New Tail

↓

Break Circle

↓

Return New Head
```

--- 