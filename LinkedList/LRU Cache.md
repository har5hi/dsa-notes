# LeetCode 146 - LRU Cache

---

# Problem Statement

Design a data structure that follows the constraints of a **Least Recently Used (LRU) Cache**.

Implement the `LRUCache` class:

- `LRUCache(int capacity)` → Initialize the cache.
- `get(key)` → Return the value if the key exists, otherwise return `-1`.
- `put(key, value)` → Insert or update the key.
- If the cache exceeds its capacity, remove the **Least Recently Used (LRU)** item.

Both operations should run in **O(1)** time.

---

## Example

```text
Input

LRUCache(2)

put(1,1)
put(2,2)
get(1)
put(3,3)
get(2)
put(4,4)
get(1)
get(3)
get(4)
```

Output

```text
null
null
null
1
null
-1
null
-1
3
4
```

---

# Brute Force Approach

Use an array or list.

For every `get()`, search the key.

Move it to the front.

For `put()`, insert/update.

If full, remove the last element.

---

### Time Complexity

```
get  -> O(n)

put  -> O(n)
```

Not acceptable.

---

# Optimal Intuition

We need two things:

### 1. Find a key quickly

Use

```
HashMap

key -> node
```

Lookup becomes

```
O(1)
```

---

### 2. Maintain usage order

Use a

```
Doubly Linked List
```

Most Recently Used (MRU)

```
Head
```

Least Recently Used (LRU)

```
Tail
```

Whenever a key is accessed, move it to the front.

Whenever capacity is exceeded, remove the node before the tail.

---

# Why Doubly Linked List?

Suppose we access

```
5
```

which is in the middle.

Need to remove it in O(1).

Singly Linked List cannot do this.

Doubly Linked List allows

```
prev

next
```

updates in O(1).

---

# Data Structures

```
HashMap

key -> node
```

```
Doubly Linked List

Head <-> ... <-> Tail
```

---

# Node Structure

```cpp
class Node{

public:

    int key;
    int value;
    Node* prev;
    Node* next;

    Node(int k,int v){
        key=k;
        value=v;
        prev=NULL;
        next=NULL;
    }
};
```

---

# DLL Helper Functions

## Insert at Front

```cpp
void insert(Node* node){
    node->next=head->next;
    node->prev=head;
    head->next->prev=node;
    head->next=node;
}
```

---

## Delete Node

```cpp
void remove(Node* node){
    node->prev->next=node->next;
    node->next->prev=node->prev;
}
```

---

# Complete Code

```cpp
class Node{
public:
    int key, value;
    Node *prev, *next;

    Node(int k, int v){
        key = k;
        value = v;
        prev = next = NULL;
    }
};

class LRUCache {
public:

    int capacity;
    unordered_map<int, Node*> mp;

    Node *head, *tail;

    LRUCache(int cap) {

        capacity = cap;

        head = new Node(-1,-1);
        tail = new Node(-1,-1);

        head->next = tail;
        tail->prev = head;
    }

    void remove(Node* node){

        node->prev->next = node->next;
        node->next->prev = node->prev;
    }

    void insert(Node* node){

        node->next = head->next;
        node->prev = head;

        head->next->prev = node;
        head->next = node;
    }

    int get(int key) {

        if(mp.find(key) == mp.end())
            return -1;

        Node* node = mp[key];

        remove(node);
        insert(node);

        return node->value;
    }

    void put(int key, int value) {

        if(mp.find(key) != mp.end()){

            Node* node = mp[key];

            node->value = value;

            remove(node);
            insert(node);

            return;
        }

        if(mp.size() == capacity){

            Node* lru = tail->prev;

            remove(lru);

            mp.erase(lru->key);

            delete lru;
        }

        Node* node = new Node(key,value);
        insert(node);
        mp[key] = node;
    }
};
```

---

# Time Complexity

| Operation | Complexity |
|-----------|-----------:|
| get() | O(1) |
| put() | O(1) |

---

# Space Complexity

```
O(capacity)
```

---

# Interview Tips

The interviewer is looking for this combination:

- **HashMap** → O(1) lookup.
- **Doubly Linked List** → O(1) insertion, deletion, and reordering.

Neither data structure alone can satisfy both requirements.

---