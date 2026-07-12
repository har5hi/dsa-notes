# Linked List Basics (C++) 🚀

> These are the absolute basics you should know before solving Linked List questions on LeetCode.

---

# What is a Linked List?

A **Linked List (LL)** is a linear data structure where each element is stored in a **node**.

Each node contains:
1. **Data (value)**
2. **Pointer to the next node**

Unlike arrays:
- Array elements are stored continuously in memory.
- Linked List nodes can be anywhere in memory and are connected using pointers.

Example:

```
Array

10 20 30 40
|__|__|__|__|


Linked List

+------+    +------+    +------+    +------+
|10| o----->|20| o----->|30| o----->|40|NULL
+------+    +------+    +------+    +------+
```

---

# Node Structure

```cpp
class Node {
public:
    int data;
    Node* next;

    Node(int data1) {
        data = data1;
        next = nullptr;
    }
};
```

## Explanation

```cpp
int data;
```

Stores the value of the node.

Example:

```
Node
------
data = 15
```

---

```cpp
Node* next;
```

Stores the address of the next node.

Example

```
10 -----> 20 -----> 30

next points to the next node.
```

---

```cpp
Node(int data1)
```

Constructor.

Whenever a new node is created,

```cpp
Node* temp = new Node(5);
```

it automatically stores

```
data = 5
next = nullptr
```

---

# What is Head?

The **head** stores the address of the first node.

```
head
 |
 V
10 -> 20 -> 30 -> NULL
```

Without head, we lose access to the linked list.

---

# What is nullptr?

The last node points to nothing.

```
10 -> 20 -> 30 -> nullptr
```

nullptr means

"There is no next node."

---

# Creating a Single Node

```cpp
Node* head = new Node(10);
```

Memory

```
head
 |
 V
+-------+
|10|NULL|
+-------+
```

---

# Converting an Array to a Linked List

## Code

```cpp
class Node {
public:
    int data;
    Node* next;

    Node(int data1) {
        data = data1;
        next = nullptr;
    }
};

Node* convertArr2LL(vector<int> &arr) {

    Node* head = new Node(arr[0]);
    Node* mover = head;

    for(int i = 1; i < arr.size(); i++) {

        Node* temp = new Node(arr[i]);
        mover->next = temp;
        mover = temp;
    }
    return head;
}
```

---

## Dry Run

Array

```
[2,4,6,8]
```

---

### Step 1

```cpp
Node* head = new Node(arr[0]);
```

Creates

```
head
 |
 V
2 -> NULL
```

---

### Step 2

```cpp
Node* mover = head;
```

```
head
 |
 V
2 -> NULL
^
|
mover
```

Mover is just another pointer used for building the list.

---

### Iteration 1

```cpp
temp = new Node(4);
```

```
4 -> NULL
```

Connect

```cpp
mover->next = temp;
```

```
2 ------->4
```

Move mover

```cpp
mover = temp;
```

```
head

2 -> 4

     ^
     |
   mover
```

---

### Iteration 2

Create node

```
6
```

Connect

```
2 -> 4 -> 6
```

Move mover

```
2 -> 4 -> 6
          ^
          |
       mover
```

---

### Iteration 3

```
2 -> 4 -> 6 -> 8
               ^
             mover
```

Return head.

---

## Time Complexity

```
O(N)
```

---

## Space Complexity

```
O(N)
```

because N new nodes are created.

---

# Traversing a Linked List

## Code

```cpp
void traverse(Node* head) {

    Node* temp = head;

    while(temp != nullptr) {

        cout << temp->data << " ";

        temp = temp->next;
    }
}
```

---

## Explanation

Start from head.

```
head

10 -> 20 -> 30 -> NULL
```

Create

```cpp
Node* temp = head;
```

Never move head.

Always move temp.

---

### Iteration 1

```
temp ->10
```

Print

```
10
```

Move

```cpp
temp = temp->next;
```

Now

```
20
```

---

### Iteration 2

Print

```
20
```

Move to

```
30
```

---

### Iteration 3

Print

```
30
```

Move

```
NULL
```

Loop ends.

---

## Time Complexity

```
O(N)
```

---

## Space Complexity

```
O(1)
```

---

# Length of a Linked List

## Code

```cpp
int length(Node* head) {

    int cnt = 0;

    Node* temp = head;

    while(temp != nullptr) {

        cnt++;

        temp = temp->next;
    }

    return cnt;
}
```

---

## Explanation

Every time we visit a node,

```
cnt++
```

Example

```
10 -> 20 -> 30 -> 40
```

Visit

```
10
cnt = 1
```

Visit

```
20
cnt = 2
```

Visit

```
30
cnt = 3
```

Visit

```
40
cnt = 4
```

Return

```
4
```

---

## Time Complexity

```
O(N)
```

---

## Space Complexity

```
O(1)
```

---

# Search in a Linked List

## Code

```cpp
bool search(Node* head, int target) {

    Node* temp = head;

    while(temp != nullptr) {

        if(temp->data == target)
            return true;

        temp = temp->next;
    }

    return false;
}
```

---

## Dry Run

```
10 -> 25 -> 40 -> 70

Target = 40
```

Visit

```
10

10 == 40 ?

No
```

Move

```
25
```

No

Move

```
40
```

Yes

Return

```
true
```

---

## Time Complexity

```
O(N)
```

Worst case: target is absent or at the last node.

---

## Space Complexity

```
O(1)
```

---

# Important Pointers

## head

Always points to the first node.

```
head
 |
10 -> 20 -> 30
```

---

## temp

Used for traversal.

```
temp

10 -> 20 -> 30
```

We move temp.

Never move head.

---

## mover

Used while creating the linked list.

```
head

10 -> 20 -> 30

           ^
         mover
```

---

## prev

Used during deletion/reversal.

```
prev

10 -> 20 -> 30
```

---

## curr

Current node.

```
curr

20
```

---

## next

Stores next node before changing pointers.

```
next

30
```

---

# Why Don't We Move Head?

Wrong

```cpp
while(head != nullptr) {

    head = head->next;
}
```

Now

```
head = NULL
```

Entire linked list is lost.

Always do

```cpp
Node* temp = head;
```

instead.

---

# Time Complexities

| Operation | Time |
|-----------|------|
| Traversal | O(N) |
| Search | O(N) |
| Length | O(N) |
| Insert at Head | O(1) |
| Delete Head | O(1) |
| Insert at Tail (without tail pointer) | O(N) |
| Delete Tail | O(N) |
| Access kth Node | O(N) |

---

# Interview Tips ⭐

1. Draw the linked list before coding.
2. Keep track of `head`, `temp`, `prev`, `curr`, and `next`.
3. Never lose the head pointer.
4. Check `nullptr` before accessing `next`.
5. Dry run pointer changes on paper before implementing.
6. Most Linked List problems are solved using one of these techniques:
   - Traversal
   - Two Pointers (Slow & Fast)
   - Reversing a List
   - Dummy Node
   - Recursion
7. If you're changing links (`next` pointers), always think about saving the next node first to avoid losing the rest of the list.

---