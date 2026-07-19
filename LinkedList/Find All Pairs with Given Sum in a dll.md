# Find All Pairs with Given Sum in a Doubly Linked List

---

# Problem Statement

Given a **sorted Doubly Linked List** and a target value `k`, find all pairs of nodes whose sum is equal to `k`.

Return all such pairs.

> **Note:** The linked list is sorted in ascending order.

---

# Key Observation ⭐

The DLL is **already sorted**.

Instead of checking every possible pair,

we can use the **Two Pointer Technique**.

One pointer starts from

```
Head
```

Another pointer starts from

```
Tail
```

Move them according to the current sum.

---

# Approach 1 : Brute Force (Nested Traversal)

## Intuition

For every node, check all nodes after it.

If their sum equals `k`, store the pair.

---

# Code

```cpp
class Solution {
public:
    vector<pair<int,int>> findPairsWithGivenSum(Node *head, int k) {

        vector<pair<int,int>> ans;

        for(Node* first = head; first != nullptr; first = first->next){

            for(Node* second = first->next; second != nullptr; second = second->next){

                if(first->data + second->data == k){

                    ans.push_back({first->data, second->data});
                }
            }
        }

        return ans;
    }
};
```

---

# Time Complexity

```
O(N²)
```

---

# Space Complexity

```
O(1)
```

(Excluding the output array.)

---

# Approach 2 : Optimal (Two Pointers)

## Intuition ⭐⭐⭐

Use two pointers.

```
Left = Head

Right = Tail
```

Compute

```
sum = left->data + right->data
```

Three cases arise.

### Case 1

If

```
sum == k
```

Store the pair.

Move both pointers.

---

### Case 2

If

```
sum < k
```

Move

```
Left++
```

because we need a larger sum.

---

### Case 3

If

```
sum > k
```

Move

```
Right--
```

because we need a smaller sum.

---

# Code

```cpp
class Solution {
public:
    vector<pair<int,int>> findPairsWithGivenSum(Node *head, int k) {

        vector<pair<int,int>> ans;

        if(head == nullptr)
            return ans;

        Node* left = head;
        Node* right = head;

        while(right->next){
            right = right->next;
        }

        while(left != right && left->prev != right){

            int sum = left->data + right->data;

            if(sum == k){

                ans.push_back({left->data, right->data});

                left = left->next;
                right = right->prev;
            }

            else if(sum < k){

                left = left->next;
            }

            else{

                right = right->prev;
            }
        }

        return ans;
    }
};
```

---

# Time Complexity

```
O(N)
```

One traversal from both ends.

---

# Space Complexity

```
O(1)
```

(Excluding the output array.)

---

# Comparison

| Approach | Time | Space | Idea |
|----------|------|-------|------|
| Nested Traversal | O(N²) | O(1) | Check every possible pair |
| Two Pointers | O(N) | O(1) | Use sorted property of DLL |

---

# Why Two Pointers Work?

Because the DLL is sorted.

```
Smallest Value

↓

Head

......

Tail

↓

Largest Value
```

If the sum is too small, we must increase it.

If the sum is too large, we must decrease it.

This is exactly what the two pointers do.

---