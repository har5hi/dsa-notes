# Find the Length of the Loop in a Linked List

---

# Problem Statement

Given the `head` of a linked list,

return the **length of the loop (cycle)**.

If there is **no loop**, return **0**.

---

# Approach 1: Brute Force (HashMap)

## Intuition

Store each node along with the **step number** at which it was visited.
When a node is visited again,

```
Loop Length = Current Step - First Visit Step
```

---

# Code

```cpp
class Solution {
public:
    int countNodesinLoop(Node *head) {

        unordered_map<Node*, int> mp;

        Node* temp = head;

        int timer = 0;

        while(temp){

            if(mp.find(temp) != mp.end()){

                return timer - mp[temp];
            }

            mp[temp] = timer;

            timer++;

            temp = temp->next;
        }

        return 0;
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

First,

detect whether a cycle exists.

If

```
Slow == Fast
```

a cycle is found.

Now,

keep one pointer fixed.

Move the other pointer until it comes back to the same node.

Count the number of steps.

That count is the **length of the loop**.

---

# Code

```cpp
class Solution {
public:

    int findLength(Node* slow){

        Node* temp = slow;

        int cnt = 1;

        temp = temp->next;

        while(temp != slow){

            cnt++;

            temp = temp->next;
        }

        return cnt;
    }

    int countNodesinLoop(Node *head) {

        Node* slow = head;
        Node* fast = head;

        while(fast != nullptr && fast->next != nullptr){

            slow = slow->next;

            fast = fast->next->next;

            if(slow == fast){

                return findLength(slow);
            }
        }

        return 0;
    }
};
```

---

# Time Complexity

```
O(N)
```

- Detecting the cycle → `O(N)`
- Counting the cycle length → `O(C)` where `C` is the cycle length.

Since `C ≤ N`,

Overall complexity remains

```
O(N)
```

---

# Space Complexity

```
O(1)
```

---

# Why is Floyd Better?

| Brute | Optimal |
|--------|----------|
| HashMap | Floyd's Algorithm |
| O(N) Space | O(1) Space |
| Easier | Interview Favourite |

---