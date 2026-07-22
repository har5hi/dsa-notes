# Remove Duplicates from Sorted Doubly Linked List

---

# Problem Statement

Given a **sorted Doubly Linked List (DLL)**, remove all duplicate nodes so that every element appears only once.

Return the head of the modified linked list.

> **Note:** Since the list is sorted, duplicate values always appear consecutively.

---

# Approach : Traverse and Delete (Optimal)

## Intuition ⭐⭐⭐

Traverse the DLL.

At every node,

```
Current

↓

Next
```

If

```
Current Value == Next Value
```

Delete the next node.

Otherwise,

move to the next node.

---

# Complete C++ Code (From Scratch)

```cpp
#include <iostream>
using namespace std;

class Node{
public:
    int data;
    Node* prev;
    Node* next;

    Node(int val){
        data = val;
        prev = nullptr;
        next = nullptr;
    }
};

// Insert at End
void insertAtEnd(Node* &head, int val){

    Node* newNode = new Node(val);

    if(head == nullptr){
        head = newNode;
        return;
    }

    Node* temp = head;

    while(temp->next){
        temp = temp->next;
    }

    temp->next = newNode;
    newNode->prev = temp;
}

// Print DLL
void printDLL(Node* head){

    while(head){
        cout << head->data;

        if(head->next)
            cout << " <-> ";

        head = head->next;
    }

    cout << endl;
}

// Remove Duplicates
Node* removeDuplicates(Node* head){

    if(head == nullptr)
        return head;

    Node* curr = head;

    while(curr && curr->next){

        if(curr->data == curr->next->data){

            Node* duplicate = curr->next;

            curr->next = duplicate->next;

            if(duplicate->next){
                duplicate->next->prev = curr;
            }

            delete duplicate;
        }
        else{

            curr = curr->next;
        }
    }

    return head;
}

int main(){

    Node* head = nullptr;

    insertAtEnd(head,1);
    insertAtEnd(head,2);
    insertAtEnd(head,2);
    insertAtEnd(head,3);
    insertAtEnd(head,4);
    insertAtEnd(head,4);
    insertAtEnd(head,5);

    cout << "Original DLL : ";
    printDLL(head);

    head = removeDuplicates(head);

    cout << "After Removing Duplicates : ";
    printDLL(head);

    return 0;
}
```

---

# Time Complexity

```
O(N)
```

Each node is visited at most once.

---

# Space Complexity

```
O(1)
```

Only constant extra space is used.

---