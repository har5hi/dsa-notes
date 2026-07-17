# 234. Palindrome Linked List

---

# Problem Statement

Given the `head` of a singly linked list, return **true** if it is a palindrome, otherwise return **false**.

A palindrome is a sequence that reads the same **forward** and **backward**.

---

# Approach 1: Brute Force (Using Stack)

## Intuition

Store all node values inside a stack.

Traverse the linked list again.

For every node,

- compare the current node value with the top of the stack.
- if they are different, return `false`.

Since a stack follows **LIFO (Last In First Out)**,

the values come out in reverse order.

If all values match, the linked list is a palindrome.

---

# Code

```cpp
class Solution {
public:
    bool isPalindrome(ListNode* head) {

        stack<int> st;

        ListNode* temp = head;

        // Push all values into the stack
        while(temp){

            st.push(temp->val);

            temp = temp->next;
        }

        temp = head;

        // Compare values
        while(temp){

            if(temp->val != st.top()){
                return false;
            }

            st.pop();

            temp = temp->next;
        }

        return true;
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

# Approach 2: Optimal (Reverse Second Half)

## Intuition ⭐⭐⭐

Instead of using extra space, reverse only the **second half** of the linked list.
Now compare

- First Half
- Reversed Second Half

If all values match, the linked list is a palindrome.
Finally, reverse the second half again to restore the original linked list.

---

# Steps

```
Find Middle

↓

Reverse Second Half

↓

Compare Both Halves

↓

Restore Original List

↓

Return Answer
```

---

# Why Do We Reverse Only the Second Half?

Reversing the first half would disconnect the head and make comparison harder.
By reversing only the second half, we can compare both halves directly using two pointers.

---

# Code

```cpp
class Solution {
public:

    ListNode* reverse(ListNode* head){

        ListNode* prev = nullptr;
        ListNode* curr = head;

        while(curr){

            ListNode* next = curr->next;

            curr->next = prev;

            prev = curr;

            curr = next;
        }

        return prev;
    }

    bool isPalindrome(ListNode* head) {

        if(head == nullptr || head->next == nullptr){
            return true;
        }

        // Find middle
        ListNode* slow = head;
        ListNode* fast = head;

        while(fast->next && fast->next->next){

            slow = slow->next;

            fast = fast->next->next;
        }

        // Reverse second half
        ListNode* secondHead = reverse(slow->next);

        // Compare both halves
        ListNode* first = head;
        ListNode* second = secondHead;

        while(second){

            if(first->val != second->val){

                // Restore original list
                slow->next = reverse(secondHead);

                return false;
            }

            first = first->next;

            second = second->next;
        }

        // Restore original list
        slow->next = reverse(secondHead);

        return true;
    }
};
```

---

# Time Complexity

```
O(N)
```

- Finding the middle → `O(N)`
- Reversing the second half → `O(N)`
- Comparing halves → `O(N)`
- Restoring the list → `O(N)`

Overall complexity is still

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

| Brute Force | Optimal |
|--------------|----------|
| Stack | Reverse Second Half |
| O(N) Space | O(1) Space |
| Easy to understand | Interview Favourite |
| Doesn't modify list | Restore list after checking |

---