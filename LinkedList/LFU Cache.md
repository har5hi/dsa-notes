# LeetCode 460 - LFU Cache

---

# Problem Statement

Design and implement a data structure for a **Least Frequently Used (LFU) Cache**.

Implement the `LFUCache` class:

- `LFUCache(int capacity)` → Initializes the cache.
- `get(key)` → Returns the value if the key exists, otherwise returns `-1`.
- `put(key, value)` → Inserts or updates the key.
- When the cache reaches capacity, remove the **Least Frequently Used** key.
- If multiple keys have the same frequency, remove the **Least Recently Used (LRU)** among them.

Both operations should run in **O(1)** average time.

---

## Example

```text
Input

LFUCache(2)

put(1,1)
put(2,2)
get(1)
put(3,3)
get(2)
get(3)
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
3
null
-1
3
4
```

---

# Brute Force Approach

Store all elements.

For every insertion when cache is full,

- Find the minimum frequency.
- If multiple keys have the same frequency,
  remove the least recently used.

---

### Time Complexity

```
O(n)
```

for every insertion.

Not acceptable.

---

# Optimal Intuition

Unlike LRU,

we must track

- Frequency
- Recency within the same frequency

So one Doubly Linked List is not enough.

---

We use

```
HashMap

key → node
```

AND

```
HashMap

frequency → Doubly Linked List
```

---

# Node Structure

Each node stores

```text
key

value

frequency

prev

next
```

---

# Why Multiple DLLs?

Suppose

```
1(freq=1)

2(freq=2)

3(freq=2)

4(freq=5)
```

When frequency changes, the node must move

```
Freq 2 DLL

↓

Freq 3 DLL
```

Hence,

each frequency has its own DLL.

---

# Complete Code

```cpp
class Node {
public:
    int key, value, freq;
    Node *prev, *next;

    Node(int k, int v) {
        key = k;
        value = v;
        freq = 1;
        prev = next = nullptr;
    }
};

class List {
public:
    Node *head, *tail;
    int size;

    List() {
        head = new Node(-1, -1);
        tail = new Node(-1, -1);

        head->next = tail;
        tail->prev = head;

        size = 0;
    }

    void addFront(Node* node) {

        node->next = head->next;
        node->prev = head;

        head->next->prev = node;
        head->next = node;

        size++;
    }

    void remove(Node* node) {

        node->prev->next = node->next;
        node->next->prev = node->prev;

        size--;
    }

    Node* removeLast() {

        if(size==0)
            return nullptr;

        Node* node = tail->prev;

        remove(node);

        return node;
    }
};

class LFUCache {
public:

    int capacity;
    int minFreq;

    unordered_map<int,Node*> keyNode;
    unordered_map<int,List*> freqList;

    LFUCache(int capacity) {

        this->capacity = capacity;

        minFreq = 0;
    }

    void updateFreq(Node* node){

        int freq = node->freq;

        freqList[freq]->remove(node);

        if(freq == minFreq &&
           freqList[freq]->size == 0)
            minFreq++;

        node->freq++;

        if(freqList.find(node->freq) == freqList.end())
            freqList[node->freq] = new List();

        freqList[node->freq]->addFront(node);
    }

    int get(int key) {

        if(keyNode.find(key)==keyNode.end())
            return -1;

        Node* node = keyNode[key];

        updateFreq(node);

        return node->value;
    }

    void put(int key, int value) {

        if(capacity==0)
            return;

        if(keyNode.find(key)!=keyNode.end()){

            Node* node = keyNode[key];

            node->value = value;

            updateFreq(node);

            return;
        }

        if(keyNode.size()==capacity){

            Node* node =
            freqList[minFreq]->removeLast();

            keyNode.erase(node->key);

            delete node;
        }

        Node* node = new Node(key,value);

        minFreq = 1;

        if(freqList.find(1)==freqList.end())
            freqList[1]=new List();

        freqList[1]->addFront(node);

        keyNode[key]=node;
    }
};
```

---

# Why is `minFreq` Needed?

Suppose

```
Freq1

(empty)

Freq2

4

5

Freq3

8
```

Without `minFreq`,

you'd need to search all frequencies to know which one to evict.

With

```
minFreq
```

you immediately know where to remove from.

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

Think of LFU as an extension of LRU:

- **LRU:** One Doubly Linked List ordered by recent usage.
- **LFU:** Multiple Doubly Linked Lists, one for each frequency.

The key insight is maintaining:

- `key → node`
- `frequency → DLL`
- `minFreq`

Together, these guarantee **O(1)** operations.

---