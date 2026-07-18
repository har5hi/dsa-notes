# 148. Sort List

---

# Problem Statement

Given the `head` of a linked list, sort the linked list in **ascending order** and return the sorted list.

> **Constraint:** The expected solution runs in **O(N log N)** time.

---

# Approach 1 : Brute Force (Store in Array)

## Intuition

Store all node values in a vector.

Sort the vector.

Traverse the linked list again and overwrite each node's value.

Although simple, it uses extra memory.

---

# Algorithm

1. Traverse the linked list.
2. Store all values in a vector.
3. Sort the vector.
4. Traverse the list again.
5. Replace node values using the sorted vector.
6. Return the head.

---

# Code

```cpp
class Solution {
public:
    ListNode* sortList(ListNode* head) {

        if(head == nullptr)
            return head;

        vector<int> nums;

        ListNode* temp = head;

        while(temp){
            nums.push_back(temp->val);
            temp = temp->next;
        }

        sort(nums.begin(), nums.end());

        temp = head;

        int i = 0;

        while(temp){
            temp->val = nums[i++];
            temp = temp->next;
        }

        return head;
    }
};
```

---

# Time Complexity

```
O(N log N)
```

Sorting dominates.

---

# Space Complexity

```
O(N)
```

Extra vector is used.

---

# Approach 2 : Optimal (Merge Sort)

## Intuition ⭐⭐⭐

Merge Sort works in three steps.

```
Divide

↓

Sort Left Half

↓

Sort Right Half

↓

Merge
```

Instead of shifting elements,

we rearrange pointers.

This makes Merge Sort ideal for Linked Lists.

---

# Algorithm

### Merge Sort

1. If list has 0 or 1 node, return it.
2. Find the middle.
3. Split the list.
4. Sort left half.
5. Sort right half.
6. Merge the two sorted lists.

---

### Merge Two Sorted Lists

1. Create a dummy node.
2. Compare both nodes.
3. Insert the smaller node.
4. Move that pointer.
5. Attach the remaining list.

---

# Code

```cpp
class Solution {
public:

    ListNode* findMiddle(ListNode* head){

        ListNode* slow = head;
        ListNode* fast = head->next;

        while(fast != nullptr && fast->next != nullptr){
            slow = slow->next;
            fast = fast->next->next;
        }

        return slow;
    }

    ListNode* merge(ListNode* left, ListNode* right){

        ListNode dummy(-1);
        ListNode* temp = &dummy;

        while(left != nullptr && right != nullptr){

            if(left->val <= right->val){
                temp->next = left;
                left = left->next;
            }
            else{
                temp->next = right;
                right = right->next;
            }

            temp = temp->next;
        }

        if(left)
            temp->next = left;

        else
            temp->next = right;

        return dummy.next;
    }

    ListNode* sortList(ListNode* head) {

        if(head == nullptr || head->next == nullptr)
            return head;

        ListNode* middle = findMiddle(head);

        ListNode* rightHead = middle->next;

        middle->next = nullptr;

        ListNode* left = sortList(head);

        ListNode* right = sortList(rightHead);

        return merge(left, right);
    }
};
```

---

# Time Complexity

Finding Middle

```
O(N)
```

performed over

```
log N
```

levels.

Overall

```
O(N log N)
```

---

# Space Complexity

```
O(log N)
```

Recursive call stack.

No extra array is used.

---

# Comparison

| Approach | Time | Space | Idea |
|----------|------|-------|------|
| Store Values in Vector | O(N log N) | O(N) | Sort values and overwrite nodes |
| Merge Sort | O(N log N) | O(log N) | Sort by rearranging pointers |

---