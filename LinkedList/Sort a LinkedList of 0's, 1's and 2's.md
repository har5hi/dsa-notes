# Sort a Linked List of 0's, 1's and 2's

---

# Problem Statement

Given the head of a linked list containing only **0s, 1s, and 2s**, sort the linked list in non-decreasing order.

Return the head of the sorted linked list.

---

# Approach 1 : Brute Force (Count Frequencies)

## Intuition

Since the linked list contains only **0, 1, and 2**, count the frequency of each value.
Traverse the list again and overwrite the node values according to the counts.

---

# Algorithm

1. Traverse the linked list.
2. Count the number of `0`s, `1`s, and `2`s.
3. Traverse the list again.
4. Fill all `0`s first.
5. Then fill all `1`s.
6. Finally fill all `2`s.
7. Return the head.

---

# Code

```cpp
class Solution {
public:
    Node* segregate(Node* head) {

        int zero = 0;
        int one = 0;
        int two = 0;

        Node* temp = head;

        while(temp){

            if(temp->data == 0)
                zero++;

            else if(temp->data == 1)
                one++;

            else
                two++;

            temp = temp->next;
        }

        temp = head;

        while(zero--){
            temp->data = 0;
            temp = temp->next;
        }

        while(one--){
            temp->data = 1;
            temp = temp->next;
        }

        while(two--){
            temp->data = 2;
            temp = temp->next;
        }

        return head;
    }
};
```

---

# Time Complexity

```
O(N)
```

Two traversals.

---

# Space Complexity

```
O(1)
```

---

# Approach 2 : Optimal (Three Separate Lists)

## Intuition ⭐⭐⭐

Instead of changing node values, create three linked lists.

```
Zero List

One List

Two List
```

Traverse the original list once.
Attach every node to its respective list.
Finally, connect the three lists together.
No node values are modified.
Only pointers are changed.

---

# Algorithm

1. Create three dummy nodes.
2. Maintain tails for each list.
3. Traverse the original list.
4. Attach each node to the correct list.
5. Join Zero → One → Two.
6. Make the last node point to `NULL`.
7. Return the head of the zero list.

---

# Code

```cpp
class Solution {
public:
    Node* segregate(Node* head) {

        Node zeroDummy(-1);
        Node oneDummy(-1);
        Node twoDummy(-1);

        Node* zero = &zeroDummy;
        Node* one = &oneDummy;
        Node* two = &twoDummy;

        Node* temp = head;

        while(temp){

            if(temp->data == 0){
                zero->next = temp;
                zero = zero->next;
            }

            else if(temp->data == 1){
                one->next = temp;
                one = one->next;
            }

            else{
                two->next = temp;
                two = two->next;
            }

            temp = temp->next;
        }

        zero->next = (oneDummy.next) ? oneDummy.next : twoDummy.next;

        one->next = twoDummy.next;

        two->next = nullptr;

        return zeroDummy.next;
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

Only three dummy nodes are used.

---

# Comparison

| Approach | Time | Space | Modifies Data? |
|----------|------|-------|----------------|
| Counting | O(N) | O(1) | ✅ Yes |
| Three Lists | O(N) | O(1) | ❌ No |

The second approach is generally preferred because it preserves the original node values and rearranges only the pointers.

---

# Interview Tips ⭐

This problem is similar to the **Dutch National Flag Algorithm** for arrays.

For linked lists, swapping nodes is difficult, so we create three separate lists and connect them.

Pattern:

```
Create Three Lists

↓

Append Nodes

↓

Join Lists
```

---

# Pattern Used

```
Dummy Nodes

+

Pointer Manipulation

+

Linked List Partitioning
```

---