# Add 1 to a Number Represented by Linked List

---

# Problem Statement

A number is represented as a linked list, where each node contains a single digit.

The **most significant digit (MSD)** comes first. Add **1** to the number and return the head of the updated linked list.

---

# Key Observation ⭐

Since the **Most Significant Digit comes first**,

we cannot directly start adding from the head.

We need to process the digits from **right to left**.

---

# Approach 1 : Brute Force (Reverse the List)

## Intuition

Reverse the linked list.

Now the Least Significant Digit becomes the head.

Add `1` exactly like normal addition.

Finally,

reverse the linked list again.

---

# Algorithm

1. Reverse the linked list.
2. Add `1` to the first node.
3. Handle carry.
4. If carry remains,
   create a new node.
5. Reverse the list again.
6. Return the head.

---

# Code

```cpp
class Solution {
public:

    Node* reverse(Node* head){

        Node* prev = nullptr;
        Node* curr = head;

        while(curr){

            Node* nextNode = curr->next;
            curr->next = prev;
            prev = curr;
            curr = nextNode;
        }

        return prev;
    }

    Node* addOne(Node* head){

        head = reverse(head);

        Node* temp = head;
        int carry = 1;

        while(temp && carry){

            int sum = temp->data + carry;

            temp->data = sum % 10;
            carry = sum / 10;

            if(carry && temp->next == nullptr){
                temp->next = new Node(0);
            }

            temp = temp->next;
        }

        return reverse(head);
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
O(1)
```

---

# Approach 2 : Optimal (Recursion)

## Intuition ⭐⭐⭐

Go to the last node using recursion.

The last node represents the Least Significant Digit. Add `1` there.

If a carry is generated, propagate it while returning from recursion.

---

# Recursive Idea

Base Case

```
NULL

↓

Return Carry = 1
```

While Returning

```
Sum = Node Value + Carry

↓

Node = Sum % 10

↓

Return Sum / 10
```

---

# Algorithm

1. Recursively move to the last node.
2. Return carry `1` from the base case.
3. Update every node while returning.
4. If carry still remains,
   create a new head.
5. Return the head.

---

# Code

```cpp
class Solution {
public:

    int helper(Node* head){

        if(head == nullptr)
            return 1;

        int carry = helper(head->next);

        int sum = head->data + carry;

        head->data = sum % 10;

        return sum / 10;
    }

    Node* addOne(Node* head){

        int carry = helper(head);

        if(carry){

            Node* newHead = new Node(carry);

            newHead->next = head;

            head = newHead;
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

Every node is visited exactly once.

---

# Space Complexity

```
O(N)
```

Recursive call stack.

---

# Comparison

| Approach | Time | Space | Idea |
|----------|------|-------|------|
| Reverse the List | O(N) | O(1) | Reverse, add 1, reverse back |
| Recursion | O(N) | O(N) | Process from the last node using recursion |

---