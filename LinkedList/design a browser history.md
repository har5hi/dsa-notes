# Design a Browser History (Linked List Implementation)

---

# Problem Statement

Design a browser history that supports the following operations:

- Visit a new webpage.
- Move back by a given number of steps.
- Move forward by a given number of steps.

Initially, the browser starts with a homepage.

Implement the following functions:

```cpp
BrowserHistory(string homepage)

visit(string url)

string back(int steps)

string forward(int steps)
```

---

# Example

```
BrowserHistory("leetcode.com")

visit("google.com")

visit("facebook.com")

visit("youtube.com")

back(1)

back(1)

forward(1)

visit("linkedin.com")

forward(2)

back(2)

back(7)
```

# How Browser History Works

Suppose

```
leetcode.com
```

Visit

```
google.com
```

History

```
leetcode <-> google
```

Visit

```
facebook.com
```

History

```
leetcode <-> google <-> facebook
```

Visit

```
youtube.com
```

History

```
leetcode <-> google <-> facebook <-> youtube
```

Current Page

```
youtube
```

---

# What Happens on Visit?

Suppose

Current

```
leetcode <-> google <-> facebook <-> youtube
                               ↑
                           Current
```

Visit

```
linkedin
```

The forward history

```
youtube
```

must disappear.

New History

```
leetcode <-> google <-> facebook <-> linkedin
```

You cannot go forward to

```
youtube
```

anymore.

---

# Data Structure

Each node stores

```cpp
class Node{
public:

    string url;

    Node* prev;

    Node* next;

    Node(string url){
        this->url = url;
        prev = nullptr;
        next = nullptr;
    }
};
```

Maintain one pointer

```
current
```

that always points to the current webpage.

---

# Algorithm

## Constructor

Create homepage node.

```
current

↓

homepage
```

---

## Visit

```
Current

↓

google
```

Disconnect

```
google -> facebook
```

Create

```
linkedin
```

Connect

```
google <-> linkedin
```

Move

```
current
```

to

```
linkedin
```

---

## Back

While

```
prev exists
```

move left.

---

## Forward

While

```
next exists
```

move right.

---

# Code

```cpp
class Node{
public:

    string url;
    Node* prev;
    Node* next;

    Node(string url){
        this->url = url;
        prev = nullptr;
        next = nullptr;
    }
};

class BrowserHistory {

    Node* current;

public:

    BrowserHistory(string homepage) {

        current = new Node(homepage);
    }

    void visit(string url) {

        Node* newNode = new Node(url);

        current->next = nullptr;

        newNode->prev = current;

        current->next = newNode;

        current = newNode;
    }

    string back(int steps) {

        while(current->prev && steps--){

            current = current->prev;
        }

        return current->url;
    }

    string forward(int steps) {

        while(current->next && steps--){

            current = current->next;
        }

        return current->url;
    }
};
```

---

# Time Complexity

### visit()

```
O(1)
```

---

### back()

```
O(steps)
```

Worst Case

```
O(N)
```

---

### forward()

```
O(steps)
```

Worst Case

```
O(N)
```

---

# Space Complexity

```
O(N)
```

One node for every visited webpage.

---

# Comparison

| Operation | Time | Space |
|-----------|------|-------|
| visit | O(1) | O(1) |
| back | O(steps) | O(1) |
| forward | O(steps) | O(1) |

Overall storage:

```
O(N)
```

---

# Interview Tips ⭐

Whenever you hear

- Browser History
- Undo / Redo
- Previous / Next navigation

think immediately of a

```
Doubly Linked List
```

because it naturally supports movement in both directions.

The core idea is:

```
Visit

↓

Create New Node

↓

Delete Forward History

↓

Move Current
```

```
Back

↓

Move using prev
```

```
Forward

↓

Move using next
```

---