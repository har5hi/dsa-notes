# 2. Add Two Numbers

---

# Problem Statement

You are given two **non-empty** linked lists representing two non-negative integers.

- The digits are stored in **reverse order**.
- Each node contains a single digit.
- Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zeros except the number `0` itself.

---

# Key Observation ⭐

Since the digits are stored in **reverse order**,

we can add them exactly the way we perform addition by hand. At every step,

```
digit = (x + y + carry) % 10

carry = (x + y + carry) / 10
```

No reversal of the linked lists is required.

---

# Approach : Simulation (Optimal)

## Intuition ⭐⭐⭐

Traverse both linked lists simultaneously.

For every pair of digits,

- Add both values.
- Include the carry from the previous step.
- Store the last digit in the answer.
- Carry the remaining value to the next iteration.

Continue until

- both lists finish, and
- there is no carry left.

---

# Algorithm

1. Create a dummy node.
2. Initialize `carry = 0`.
3. Traverse while
   - `l1` exists, or
   - `l2` exists, or
   - `carry` is non-zero.
4. Take current values from both lists.
5. Compute
   - sum
   - digit
   - carry.
6. Create a new node with the digit.
7. Move pointers.
8. Return `dummy.next`.

---

# Code

```cpp
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {

        ListNode dummy(-1);
        ListNode* temp = &dummy;

        int carry = 0;

        while(l1 != nullptr || l2 != nullptr || carry){

            int sum = carry;

            if(l1){
                sum += l1->val;
                l1 = l1->next;
            }

            if(l2){
                sum += l2->val;
                l2 = l2->next;
            }

            carry = sum / 10;

            temp->next = new ListNode(sum % 10);

            temp = temp->next;
        }

        return dummy.next;
    }
};
```

---

# Time Complexity

```
O(max(N, M))
```

where

- `N` = length of `l1`
- `M` = length of `l2`

Each node is visited once.

---

# Space Complexity

```
O(max(N, M))
```

The output linked list stores the answer.

(Extra auxiliary space apart from the output is **O(1)**.)

---

# Why Dummy Node?

Instead of handling the first node separately,

we always attach new nodes after the dummy node.

```
Dummy

↓

-1 -> 7 -> 0 -> 8
```

Finally,

return

```cpp
dummy.next;
```

This keeps the implementation simple.

---