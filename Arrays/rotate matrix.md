# LeetCode 48 - Rotate Image

## Problem Statement

You are given an `n x n` 2D matrix representing an image.

Rotate the image by **90 degrees clockwise**.

You must rotate the image **in-place**,meaning you cannot use another matrix.

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
7 4 1
8 5 2
9 6 3
```

---

# Approaches

## 1. Brute Force

### Idea

Create another matrix.

For every element:

```cpp
newMatrix[j][n-1-i] = matrix[i][j];
```

Then copy back.

### Complexity

Time:

```text
O(n²)
```

Space:

```text
O(n²)
```

Not allowed because the problem requires in-place rotation.

---

# Optimal Approach (Transpose + Reverse)

## Step 1: Transpose

Traverse only the upper triangle.

```cpp
for(int i = 0; i < n; i++)
{
    for(int j = i + 1; j < n; j++)
    {
        swap(matrix[i][j], matrix[j][i]);
    }
}
```

### Why `j = i + 1`?

Because:

```cpp
matrix[i][j]
```

and

```cpp
matrix[j][i]
```

represent the same pair.

If we traverse the whole matrix, we'll swap twice and undo our work.

---

## Step 2: Reverse Every Row

```cpp
for(int i = 0; i < n; i++)
{
    reverse(matrix[i].begin(), matrix[i].end());
}
```

This flips each row horizontally.

---

# Dry Run

Input:

```text
1 2 3
4 5 6
7 8 9
```

---

## Transpose Phase

### Swap (0,1) and (1,0)

```text
1 4 3
2 5 6
7 8 9
```

---

### Swap (0,2) and (2,0)

```text
1 4 7
2 5 6
3 8 9
```

---

### Swap (1,2) and (2,1)

```text
1 4 7
2 5 8
3 6 9
```

Matrix after transpose:

```text
1 4 7
2 5 8
3 6 9
```

---

## Reverse Rows

### Row 1

```text
1 4 7
↓
7 4 1
```

### Row 2

```text
2 5 8
↓
8 5 2
```

### Row 3

```text
3 6 9
↓
9 6 3
```

---

## Final Answer

```text
7 4 1
8 5 2
9 6 3
```

---

# Code Explanation (Line by Line)

### Get matrix size

```cpp
int n = matrix.size();
```

Stores the dimension of the square matrix.

---

### Transpose Matrix

```cpp
for(int i = 0; i < n; i++)
```

Traverse each row.

---

```cpp
for(int j = i + 1; j < n; j++)
```

Start from the next column to avoid duplicate swaps.

---

```cpp
swap(matrix[i][j], matrix[j][i]);
```

Exchange symmetric elements across the main diagonal.

---

### Reverse Every Row

```cpp
for(int i = 0; i < n; ++i)
```

Visit every row.

---

```cpp
reverse(matrix[i].begin(), matrix[i].end());
```

Reverse that row completely.

This converts the transposed matrix into the rotated matrix.

---

# Optimal Code

```cpp
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {

        int n = matrix.size();

        // Step 1: Transpose
        for(int i = 0; i < n; i++) {
            for(int j = i + 1; j < n; j++) {
                swap(matrix[i][j], matrix[j][i]);
            }
        }

        // Step 2: Reverse each row
        for(int i = 0; i < n; i++) {
            reverse(matrix[i].begin(), matrix[i].end());
        }
    }
};
```

---

# Complexity Analysis

### Time Complexity

Transpose:

```text
O(n²)
```

Reverse Rows:

```text
O(n²)
```

Overall:

```text
O(n²)
```

---

### Space Complexity

```text
O(1)
```

No extra matrix is used.

---

# Interview Explanation

"I rotate the matrix in-place using two operations. First, I transpose the matrix by swapping elements across the main diagonal. Then I reverse every row. Transposition converts rows into columns, and reversing each row produces a 90-degree clockwise rotation. This achieves O(n²) time and O(1) extra space."

---