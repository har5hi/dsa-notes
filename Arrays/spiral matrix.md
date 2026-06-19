# LeetCode 54 - Spiral Matrix

## Problem Statement

Given an `m x n` matrix, return all elements of the matrix in **spiral order**.

---

## Example

### Input

```text
1 2 3
4 5 6
7 8 9
```

### Output

```text
[1,2,3,6,9,8,7,4,5]
```

---

# Intuition

We traverse the matrix layer by layer.

For each layer:

1. Traverse Left → Right
2. Traverse Top → Bottom
3. Traverse Right → Left
4. Traverse Bottom → Top

After completing one layer, shrink the boundaries inward.

---

# Boundary Variables

We maintain four pointers:

```cpp
left = 0;
right = m - 1;
top = 0;
bottom = n - 1;
```
---

# Algorithm

Repeat while:

```cpp
top <= bottom && left <= right
```

---

## Step 1: Left → Right

Traverse top row.

```cpp
for(int i = left; i <= right; i++)
{
    ans.push_back(matrix[top][i]);
}
top++;
```

Example:

```text
1 2 3
4 5 6
7 8 9
```

Add:

```text
1 2 3
```

Move:

```cpp
top++
```

---

## Step 2: Top → Bottom

Traverse right column.

```cpp
for(int i = top; i <= bottom; i++)
{
    ans.push_back(matrix[i][right]);
}
right--;
```

Add:

```text
6 9
```

Move:

```cpp
right--
```

---

## Step 3: Right → Left

Traverse bottom row.

```cpp
if(top <= bottom)
{
    for(int i = right; i >= left; i--)
    {
        ans.push_back(matrix[bottom][i]);
    }
    bottom--;
}
```

Add:

```text
8 7
```

Move:

```cpp
bottom--
```

---

## Step 4: Bottom → Top

Traverse left column.

```cpp
if(left <= right)
{
    for(int i = bottom; i >= top; i--)
    {
        ans.push_back(matrix[i][left]);
    }
    left++;
}
```

Add:

```text
4
```

Move:

```cpp
left++
```

---

# Why Do We Need These Conditions?

## Condition 1

```cpp
if(top <= bottom)
```

Prevents traversing the same row twice.

Example:

```text
1 2 3
```

Single row matrix.

After moving top:

```cpp
top > bottom
```

Without the condition, we'd revisit the same row.

---

## Condition 2

```cpp
if(left <= right)
```

Prevents traversing the same column twice.

Example:

```text
1
2
3
```

Single column matrix.

Without the condition, duplicates would occur.

---

# Dry Run

Input:

```text
1 2 3
4 5 6
7 8 9
```

---

## Initial Boundaries

```text
top = 0
bottom = 2
left = 0
right = 2
```

Answer:

```text
[]
```

---

## Left → Right

Add:

```text
1 2 3
```

Answer:

```text
[1,2,3]
```

Update:

```cpp
top = 1
```

---

## Top → Bottom

Add:

```text
6 9
```

Answer:

```text
[1,2,3,6,9]
```

Update:

```cpp
right = 1
```

---

## Right → Left

Add:

```text
8 7
```

Answer:

```text
[1,2,3,6,9,8,7]
```

Update:

```cpp
bottom = 1
```

---

## Bottom → Top

Add:

```text
4
```

Answer:

```text
[1,2,3,6,9,8,7,4]
```

Update:

```cpp
left = 1
```

---

## Remaining Layer

Current boundaries:

```text
top = 1
bottom = 1
left = 1
right = 1
```

Only element left:

```text
5
```

Add:

```text
[1,2,3,6,9,8,7,4,5]
```

---

## Final Answer

```text
[1,2,3,6,9,8,7,4,5]
```

---

# Optimal Code

```cpp
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {

        int n = matrix.size();
        int m = matrix[0].size();

        int left = 0;
        int right = m - 1;

        int top = 0;
        int bottom = n - 1;

        vector<int> ans;

        while(top <= bottom && left <= right){

            // Left -> Right
            for(int i = left; i <= right; i++){
                ans.push_back(matrix[top][i]);
            }
            top++;

            // Top -> Bottom
            for(int i = top; i <= bottom; i++){
                ans.push_back(matrix[i][right]);
            }
            right--;

            // Right -> Left
            if(top <= bottom){
                for(int i = right; i >= left; i--){
                    ans.push_back(matrix[bottom][i]);
                }
                bottom--;
            }

            // Bottom -> Top
            if(left <= right){
                for(int i = bottom; i >= top; i--){
                    ans.push_back(matrix[i][left]);
                }
                left++;
            }
        }

        return ans;
    }
};
```

---

# Complexity Analysis

### Time Complexity

```text
O(m × n)
```

Every element is visited exactly once.

---

### Space Complexity

```text
O(1)
```

Ignoring the output array.

Output array stores all elements:

```text
O(m × n)
```
---

# Interview Explanation

"I maintain four boundaries: top, bottom, left, and right. For each layer, I traverse the top row, right column, bottom row, and left column, then shrink the boundaries inward. The extra checks `top <= bottom` and `left <= right` prevent duplicate traversals in single-row or single-column cases. Every element is visited exactly once, giving O(m*n) time complexity."

---