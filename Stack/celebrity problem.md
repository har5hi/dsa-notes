# Celebrity Problem

---

# Problem Statement

A party has `N` people labeled from `0` to `N-1`. A **celebrity** is a person who:

- **Knows nobody** at the party.
- **Everyone knows them.**

You are given an `N × N` matrix `M` where

```
M[i][j] = 1
```

means

```
Person i knows Person j
```

and

```
M[i][j] = 0
```

means

```
Person i does not know Person j
```

Find the celebrity. If no celebrity exists, return `-1`.

---

## Example

```text
Input

M =
[
[0 1 0]
[0 0 0]
[0 1 0]
]
```

Output

```text
1
```

---

### Explanation

```
0 knows 1

2 knows 1

1 knows nobody
```

Hence,

```
1
```

is the celebrity.

---

# Brute Force Approach

For every person,

check

- Does everyone know them?
- Do they know nobody?

If both conditions are true, return that person.

---

## Code

```cpp
class Solution {
public:
    int celebrity(vector<vector<int>>& M, int n) {

        for(int i = 0; i < n; i++){

            bool knowsNobody = true;
            bool knownByEveryone = true;

            // Row check
            for(int j = 0; j < n; j++){
                if(i != j && M[i][j] == 1){
                    knowsNobody = false;
                    break;
                }
            }

            // Column check
            for(int j = 0; j < n; j++){
                if(i != j && M[j][i] == 0){
                    knownByEveryone = false;
                    break;
                }
            }
            if(knowsNobody && knownByEveryone)
                return i;
        }
        return -1;
    }
};
```

---

## Time Complexity

```
O(n²)
```

---

## Space Complexity

```
O(1)
```

---

# Optimal Intuition

Instead of checking everyone, **eliminate non-celebrities one by one.**

### Key Observation

Suppose we have

```
A

B
```

If

```
A knows B
```

then

```
A
```

**cannot** be a celebrity.

---

If

```
A does NOT know B
```

then

```
B
```

cannot be a celebrity.

---

So in one comparison, one person gets eliminated.

---

# Elimination Rule

```
A knows B

↓

A cannot be celebrity
```

---

```
A doesn't know B

↓

B cannot be celebrity
```

---

After eliminating, only **one candidate** remains.
Finally, verify whether that candidate is actually a celebrity.

---

# Optimal Code (Two Pointer)

```cpp
class Solution {
public:
    int celebrity(vector<vector<int>>& M, int n) {

        int low = 0;
        int high = n - 1;

        while(low < high){
            if(M[low][high] == 1)
                low++;
            else
                high--;
        }

        int candidate = low;

        // Row Check
        for(int j = 0; j < n; j++){
            if(candidate != j &&
               M[candidate][j] == 1)
                return -1;
        }

        // Column Check
        for(int i = 0; i < n; i++){
            if(candidate != i &&
               M[i][candidate] == 0)
                return -1;
        }
        return candidate;
    }
};
```

---

# Stack Solution

Instead of two pointers, we can eliminate using a stack.

---

## Code

```cpp
class Solution {
public:
    int celebrity(vector<vector<int>>& M, int n) {

        stack<int> st;

        for(int i = 0; i < n; i++)
            st.push(i);

        while(st.size() > 1){

            int a = st.top();
            st.pop();

            int b = st.top();
            st.pop();

            if(M[a][b] == 1)
                st.push(b);
            else
                st.push(a);
        }

        int candidate = st.top();

        // Row Check
        for(int j = 0; j < n; j++){
            if(candidate != j &&
               M[candidate][j] == 1)
                return -1;
        }

        // Column Check
        for(int i = 0; i < n; i++){
            if(candidate != i &&
               M[i][candidate] == 0)
                return -1;
        }
        return candidate;
    }
};
```

---

# Time Complexity

### Two Pointer

```
Elimination → O(n)

Verification → O(n)

Total → O(n)
```

---

### Stack

```
Elimination → O(n)

Verification → O(n)

Total → O(n)
```

---

# Space Complexity

### Two Pointer

```
O(1)
```

---

### Stack

```
O(n)
```

---

# Interview Tips

The interviewer usually wants you to identify the **elimination property**:

> A single comparison always eliminates one person from being the celebrity.

This reduces the search from **O(n²)** to **O(n)**.

---