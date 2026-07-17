# 141. Linked List Cycle

---

# Problem Statement

Given the `head` of a linked list, determine whether the linked list has a **cycle** in it.
A cycle exists if some node in the list can be reached again by continuously following the `next` pointers.

Return:

- `true` → if there is a cycle.
- `false` → otherwise.

---

# Approach 1: Brute Force (Hashing)

## Intuition

While traversing the linked list, store every visited node inside a hash set.
If we visit a node that already exists in the set, there is a cycle.

---

# Code

```cpp
class Solution {
public:
    bool hasCycle(ListNode *head) {

        unordered_set<ListNode*> st;

        ListNode* temp = head;

        while(temp){

            // Node already visited
            if(st.find(temp) != st.end()){
                return true;
            }

            st.insert(temp);

            temp = temp->next;
        }
        return false;
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

HashSet stores every node.

---

# Approach 2: Optimal (Floyd's Cycle Detection Algorithm)

## Intuition ⭐⭐⭐

Use two pointers.

- Slow Pointer → moves one step.
- Fast Pointer → moves two steps.

If there is **no cycle**,

Fast eventually reaches `NULL`.

If there **is** a cycle,

Fast will eventually catch Slow.

---

# Code

```cpp
class Solution {
public:
    bool hasCycle(ListNode *head) {

        ListNode* slow = head;
        ListNode* fast = head;

        while(fast != nullptr && fast->next != nullptr){

            slow = slow->next;

            fast = fast->next->next;

            // Both pointers meet
            if(slow == fast){
                return true;
            }
        }
        return false;
    }
};
```

---

# Why is Floyd's Algorithm Better?

| Hashing | Floyd's Algorithm |
|----------|-------------------|
| Uses HashSet | Uses Two Pointers |
| O(N) Space | O(1) Space |
| Easy to understand | Most optimized |
| Extra memory required | No extra memory |

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

# Proof (Why They Meet?)

Suppose

Slow moves

```
1 step
```

Fast moves

```
2 steps
```

Inside the cycle,

Fast gains

```
1 node
```

on Slow every iteration.

Eventually,

the gap becomes

```
0
```

Hence,

they must meet.

---

# 142. Linked List Cycle II

---

# Problem Statement

Given the `head` of a linked list, return the **node where the cycle begins**.
If there is **no cycle**, return `NULL`.
> **Note:** Do **not** modify the linked list.

---

# Approach 1: Brute Force (Hashing)

## Intuition

Store every visited node inside a HashSet.

The first node that is visited **twice** is the starting node of the cycle.

---

# Code

```cpp
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {

        unordered_set<ListNode*> st;

        ListNode* temp = head;

        while(temp){

            if(st.find(temp) != st.end()){
                return temp;
            }

            st.insert(temp);

            temp = temp->next;
        }

        return nullptr;
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

---

# Approach 2: Optimal (Floyd's Cycle Detection)

## Intuition ⭐⭐⭐

This problem is an extension of **LeetCode 141**.

### Step 1

Use Slow & Fast pointers.

If they never meet,

```
No Cycle
```

Return

```
NULL
```

---

### Step 2

If they meet,

move one pointer back to `head`.

Keep the other pointer at the meeting point.

Now move **both one step at a time**.

The node where they meet again is the **starting point of the cycle**.

---

# Why Does This Work?

Suppose

```
L = Distance from head to cycle start

C = Length of cycle

x = Distance from cycle start to meeting point
```

When slow and fast meet,

mathematically,

```
L = C - x
```

(or equivalently, L differs from C - x by a multiple of C).

This means

- one pointer starting from `head`
- another starting from the meeting point

will travel the same remaining distance and meet exactly at the **cycle's starting node**.

This is the key idea behind Floyd's algorithm.

---

# Code

```cpp
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {

        ListNode* slow = head;
        ListNode* fast = head;

        // Detect cycle
        while(fast != nullptr && fast->next != nullptr){

            slow = slow->next;
            fast = fast->next->next;

            if(slow == fast){

                ListNode* entry = head;

                // Find cycle starting node
                while(entry != slow){

                    entry = entry->next;
                    slow = slow->next;
                }

                return entry;
            }
        }

        return nullptr;
    }
};
```

---

# Time Complexity

```
O(N)
```

Even though there are two phases, each pointer traverses at most `O(N)` nodes overall.

---

# Space Complexity

```
O(1)
```

---

# Comparison

| Brute Force | Optimal |
|-------------|----------|
| HashSet | Floyd's Algorithm |
| O(N) Space | O(1) Space |
| Easier | Interview Favourite |

---