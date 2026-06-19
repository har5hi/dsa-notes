# LeetCode 73 - Set Matrix Zeroes

## Problem Statement

Given an `m x n` integer matrix, if an element is `0`, set its entire row and column to `0`.

You must do it **in-place**.

### Example

Input:

```text
1 1 1
1 0 1
1 1 1
```

Output:

```text
1 0 1
0 0 0
1 0 1
```

---

# Approaches

## 1. Brute Force

### Idea

Whenever a `0` is found:

- Mark its entire row
- Mark its entire column

Since modifying immediately may affect future checks, use a special marker value (e.g. `INT_MIN`) and convert all markers to `0` later.

### Complexity

- Time: `O((m*n)*(m+n))`
- Space: `O(1)`

Not optimal.

---

## 2. Better Approach

### Idea

Store:

- Rows containing zero in a row array
- Columns containing zero in a col array

### Steps

1. Traverse matrix
2. If `matrix[i][j] == 0`
   - `row[i] = 1`
   - `col[j] = 1`
3. Traverse again
4. If row or column is marked, make cell `0`

### Complexity

- Time: `O(m*n)`
- Space: `O(m+n)`

---

# Optimal Approach (Expected Interview Solution)

## Key Observation

Instead of using separate row and column arrays:

- Use first column as row marker
- Use first row as column marker

### Markers

```text
matrix[i][0] -> Row i marker
matrix[0][j] -> Column j marker
```

Example:

```text
1 0 3
0 0 6
7 8 9
```

Here:

- Row 1 marked
- Column 1 marked

---

## Special Case

Cell:

```cpp
matrix[0][0]
```

belongs to both:

- First row
- First column

Therefore we need an extra variable:

```cpp
bool firstCol = false;
```

to track whether the first column originally contains a zero.

---

# Algorithm

## Step 1: Mark Rows and Columns

Traverse matrix.

Whenever a zero is found:

```cpp
matrix[i][0] = 0;
matrix[0][j] = 0;
```

These act as markers.

---

## Step 2: Fill Using Markers

Traverse matrix excluding first row and first column.

If:

```cpp
matrix[i][0] == 0 || matrix[0][j] == 0
```

then:

```cpp
matrix[i][j] = 0;
```

---

## Step 3: Handle First Row

If:

```cpp
matrix[0][0] == 0
```

make entire first row zero.

---

## Step 4: Handle First Column

If:

```cpp
firstCol == true
```

make entire first column zero.

---

# Dry Run

Input:

```text
1 1 1
1 0 1
1 1 1
```

---

## After Marking Phase

Found zero at `(1,1)`.

Mark:

```cpp
matrix[1][0] = 0;
matrix[0][1] = 0;
```

Matrix becomes:

```text
1 0 1
0 0 1
1 1 1
```

---

## Fill Using Markers

Cell `(1,2)`:

```cpp
matrix[1][0] == 0
```

Set:

```cpp
matrix[1][2] = 0
```

Cell `(2,1)`:

```cpp
matrix[0][1] == 0
```

Set:

```cpp
matrix[2][1] = 0
```

Matrix becomes:

```text
1 0 1
0 0 0
1 0 1
```

---

## First Row

```cpp
matrix[0][0] != 0
```

No change.

---

## First Column

```cpp
firstCol == false
```

No change.

---

## Final Answer

```text
1 0 1
0 0 0
1 0 1
```

---

# Optimal Code (C++)

```cpp
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {

        int m = matrix.size();
        int n = matrix[0].size();

        bool firstCol = false;

        // Step 1: Mark rows and columns
        for(int i = 0; i < m; i++) {

            if(matrix[i][0] == 0)
                firstCol = true;

            for(int j = 1; j < n; j++) {

                if(matrix[i][j] == 0) {
                    matrix[i][0] = 0;
                    matrix[0][j] = 0;
                }
            }
        }

        // Step 2: Fill using markers
        for(int i = 1; i < m; i++) {

            for(int j = 1; j < n; j++) {

                if(matrix[i][0] == 0 || matrix[0][j] == 0)
                    matrix[i][j] = 0;
            }
        }

        // Step 3: Handle first row
        if(matrix[0][0] == 0) {

            for(int j = 0; j < n; j++)
                matrix[0][j] = 0;
        }

        // Step 4: Handle first column
        if(firstCol) {

            for(int i = 0; i < m; i++)
                matrix[i][0] = 0;
        }
    }
};
```

---

# Complexity Analysis

### Time Complexity

```text
O(m × n)
```

Each cell is visited a constant number of times.

### Space Complexity

```text
O(1)
```

Only one extra variable (`firstCol`) is used.

---

# Interview Explanation

"I use the first row and first column as marker arrays. During the first traversal, whenever I find a zero, I mark its row and column using `matrix[i][0]` and `matrix[0][j]`. In the second traversal, these markers determine which cells become zero. Since `matrix[0][0]` cannot independently represent both the first row and first column, I use an additional boolean variable `firstCol` to track the state of the first column. This achieves O(m*n) time and O(1) extra space."

---