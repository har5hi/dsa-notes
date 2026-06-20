# LeetCode 118 & 119 — Pascal's Triangle Notes

---

# LeetCode 118: Pascal's Triangle

## Problem Statement

Given an integer `numRows`, return the first `numRows` rows of Pascal's Triangle.

### Example

Input:

```cpp
numRows = 5
```

Output:

```cpp
[
     [1],
    [1,1],
   [1,2,1],
  [1,3,3,1],
 [1,4,6,4,1]
]
```

---

### Important Rules

* First element of every row = 1
* Last element of every row = 1
* Middle elements:

```cpp
currentRow[j] =
previousRow[j-1] + previousRow[j]
```

---

# Approach

For every row:

1. Create a row of size `(rowNumber + 1)`
2. Put `1` at first and last position.
3. For middle positions:

   * Take values from previous row.
4. Add current row to answer.

---

# Algorithm

For each row `i`:

```cpp
Create vector of size i+1

row[0] = 1
row[i] = 1

For j from 1 to i-1:
    row[j] = ans[i-1][j-1] + ans[i-1][j]

Push row into answer
```

---

# Dry Run

Input:

```cpp
numRows = 5
```

---

### Row 0

Size = 1

```cpp
[1]
```

Answer:

```cpp
[
 [1]
]
```

---

### Row 1

Size = 2

```cpp
[1,1]
```

Answer:

```cpp
[
 [1],
 [1,1]
]
```

---

### Row 2

Size = 3

First and last = 1

```cpp
[1, ?, 1]
```

Middle:

```cpp
1 + 1 = 2
```

Row:

```cpp
[1,2,1]
```

Answer:

```cpp
[
 [1],
 [1,1],
 [1,2,1]
]
```

---

### Row 3

Size = 4

```cpp
[1, ?, ?, 1]
```

Middle:

```cpp
1 + 2 = 3
2 + 1 = 3
```

Row:

```cpp
[1,3,3,1]
```

---

### Row 4

```cpp
[1, ?, ?, ?, 1]
```

Middle:

```cpp
1+3 = 4
3+3 = 6
3+1 = 4
```

Row:

```cpp
[1,4,6,4,1]
```

Final Answer:

```cpp
[
 [1],
 [1,1],
 [1,2,1],
 [1,3,3,1],
 [1,4,6,4,1]
]
```

---

# Code

```cpp
class Solution {
public:
    vector<vector<int>> generate(int numRows) {

        vector<vector<int>> ans;

        for(int i = 0; i < numRows; i++) {

            vector<int> row(i + 1, 1);

            for(int j = 1; j < i; j++) {
                row[j] = ans[i - 1][j - 1] + ans[i - 1][j];
            }

            ans.push_back(row);
        }

        return ans;
    }
};
```

---

# Complexity Analysis

### Time Complexity

Outer loop runs:

```cpp
numRows times
```

Inner loop runs:

```cpp
1 + 2 + 3 + ... + (numRows-1)
```

So:

```cpp
O(numRows²)
```

---

### Space Complexity

We store entire triangle.

Total elements:

```cpp
1 + 2 + 3 + ... + numRows
```

Therefore:

```cpp
O(numRows²)
```

---

---

# LeetCode 119: Pascal's Triangle II

## Problem Statement

Given an integer `rowIndex`, return the rowIndex-th row of Pascal's Triangle.

### Example

Input:

```cpp
rowIndex = 3
```

Output:

```cpp
[1,3,3,1]
```

---

# Key Observation

We only need ONE row.

Building the whole triangle works, but wastes space.

A better way uses the mathematical relation:

## Combination Formula

Pascal Triangle values are:

```cpp
nCr
```

For row `n`:

```cpp
0C0 0C1 0C2 ...

or

nC0 nC1 nC2 ... nCn
```

Example:

Row 4

```cpp
4C0 = 1
4C1 = 4
4C2 = 6
4C3 = 4
4C4 = 1
```

Result:

```cpp
[1,4,6,4,1]
```

---

# Optimized Formula

Instead of calculating factorials:

Use:

```cpp
nCr = nC(r-1) * (n-r+1) / r
```

This generates next element from previous element.

---

# Approach

Start with:

```cpp
ans = [1]
```

Current value:

```cpp
prev = 1
```

For every position:

```cpp
next =
prev * (n-r+1) / r
```

Append next.

---

# Dry Run

Input:

```cpp
rowIndex = 4
```

Need:

```cpp
[1,4,6,4,1]
```

---

### Initially

```cpp
ans = [1]
prev = 1
```

---

### r = 1

```cpp
prev = 1*(4-1+1)/1
     = 4
```

ans:

```cpp
[1,4]
```

---

### r = 2

```cpp
prev = 4*(4-2+1)/2
     = 4*3/2
     = 6
```

ans:

```cpp
[1,4,6]
```

---

### r = 3

```cpp
prev = 6*(4-3+1)/3
     = 6*2/3
     = 4
```

ans:

```cpp
[1,4,6,4]
```

---

### r = 4

```cpp
prev = 4*(4-4+1)/4
     = 4*1/4
     = 1
```

ans:

```cpp
[1,4,6,4,1]
```

Final Answer:

```cpp
[1,4,6,4,1]
```

---

# Optimal Code

```cpp
class Solution {
public:
    vector<int> getRow(int rowIndex) {

        vector<int> ans;

        long long prev = 1;
        ans.push_back(1);

        for(int r = 1; r <= rowIndex; r++) {

            prev = prev * (rowIndex - r + 1) / r;

            ans.push_back(prev);
        }

        return ans;
    }
};
```

---

# Why Use long long?

Intermediate multiplication can exceed `int`.

Example:

```cpp
prev * (rowIndex-r+1)
```

Using `long long` prevents overflow.

---

# Complexity Analysis

### Time Complexity

Single loop:

```cpp
O(rowIndex)
```

---

### Space Complexity

Only output row stored:

```cpp
O(rowIndex)
```

---

# Interview Follow-Up

### Brute Force

Generate complete Pascal Triangle till `rowIndex`.

```cpp
Time: O(n²)
Space: O(n²)
```

---

### Optimal

Use Combination relation:

```cpp
nCr = nC(r-1) * (n-r+1) / r
```

```cpp
Time: O(n)
Space: O(n)
```

This is the most optimized solution commonly expected in interviews.