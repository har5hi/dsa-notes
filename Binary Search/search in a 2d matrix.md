# 74. Search a 2D Matrix

## Problem Statement

You are given an `n × m` matrix where:

- Each row is sorted in non-decreasing order.
- The first element of every row is greater than the last element of the previous row.

Determine whether a given:

```cpp
target
```

exists in the matrix.

Return:

```cpp
true
```

if found, otherwise:

```cpp
false
```

---

## Example 1

```cpp
Input:

matrix =

1  3  5  7
10 11 16 20
23 30 34 60

target = 3
```

Output:

```cpp
true
```

---

# Brute Force Approach

## Idea

Traverse every element.

If:

```cpp
matrix[i][j] == target
```

return:

```cpp
true
```

Otherwise:

```cpp
false
```

---

## Code

```cpp
class Solution {
public:

    bool searchMatrix(vector<vector<int>>& matrix,
                      int target) {

        int n = matrix.size();
        int m = matrix[0].size();

        for(int i = 0; i < n; i++) {

            for(int j = 0; j < m; j++) {

                if(matrix[i][j] == target)
                    return true;
            }
        }

        return false;
    }
};
```

---

## Complexity Analysis

| Complexity | Value |
|------------|--------|
| Time | O(n × m) |
| Space | O(1) |

---

# Better Approach

For every row:

Check whether target lies between:

```cpp
row.front()
```

and

```cpp
row.back()
```

If yes,

perform Binary Search on that row.

---

## Complexity

```cpp
Time = O(n + log m)
```

---

# Optimal Approach (Binary Search on Entire Matrix)

## Key Observation

The matrix is globally sorted.

Example:

```text
1   3   5   7

10 11 16 20

23 30 34 60
```

If we write it as a single array:

```text
1 3 5 7 10 11 16 20 23 30 34 60
```

It is completely sorted.

So we can directly apply Binary Search.

---

# Virtual 1D Array

Suppose:

```cpp
n = 3
m = 4
```

Indexes become:

```text
0  1  2  3
4  5  6  7
8  9 10 11
```

Notice:

```text
2D Matrix

↓

1D Sorted Array
```

No extra array is required.

---

# Mapping Formula

Suppose Binary Search gives:

```cpp
mid
```

We need to convert it back into:

```cpp
row
```

and

```cpp
column
```

---

## Row Formula

Each row contains:

```cpp
m elements
```

Therefore,

```cpp
row = mid / m
```

---

## Column Formula

The remaining elements after complete rows:

```cpp
col = mid % m
```

---

# Optimal Code

```cpp
class Solution {
public:

    bool searchMatrix(vector<vector<int>>& matrix,
                      int target) {

        int n = matrix.size();
        int m = matrix[0].size();

        int low = 0;
        int high = n * m - 1;

        while(low <= high) {

            int mid =
                low + (high - low) / 2;

            int row = mid / m;
            int col = mid % m;

            if(matrix[row][col] == target)
                return true;

            else if(matrix[row][col] < target)
                low = mid + 1;

            else
                high = mid - 1;
        }

        return false;
    }
};
```

---

# Why Does This Work?

The matrix satisfies:

```cpp
Last element of previous row < First element of next row
```

Therefore, the entire matrix behaves exactly like one sorted array.
Binary Search can be applied directly.

---

# Complexity Analysis

There are:

```cpp
n × m
```

elements.

Binary Search takes:

```cpp
log(n × m)
```

comparisons.

| Complexity | Value |
|------------|--------|
| Time | O(log(n × m)) |
| Space | O(1) |

---

# Interview Tip

This problem is often confused with **LC 240 - Search a 2D Matrix II**.

| LC 74 | LC 240 |
|--------|---------|
| Entire matrix is globally sorted | Only rows and columns are individually sorted |
| Binary Search | Staircase Search |
| Time: O(log(n×m)) | Time: O(n+m) |

--- 

# 240. Search a 2D Matrix II

## Problem Statement

You are given an `n × m` matrix where:

- Each row is sorted in increasing order.
- Each column is sorted in increasing order.

Return:

```cpp
true
```

if the target exists in the matrix, otherwise return:

```cpp
false
```

---

## Example

```cpp
Matrix =

1   4   7   11  15
2   5   8   12  19
3   6   9   16  22
10 13 14 17 24
18 21 23 26 30

target = 5
```

Output:

```cpp
true
```

---

# Why Can't We Use Binary Search Here?

Consider:

```text
1   4   7
2   5   8
3   6   9
```

Flattened array becomes:

```text
1 4 7 2 5 8 3 6 9
```

Notice:

```text
NOT SORTED
```

Therefore, Binary Search on the entire matrix is **not possible**.

---

# Brute Force Approach

## Idea

Traverse every element.

If target is found:

```cpp
return true;
```

Else:

```cpp
return false;
```

---

## Code

```cpp
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {

        int n = matrix.size();
        int m = matrix[0].size();

        for(int i = 0; i < n; i++) {
            for(int j = 0; j < m; j++) {
                if(matrix[i][j] == target)
                    return true;
            }
        }
        return false;
    }
};
```

---

## Complexity

| Complexity | Value |
|------------|--------|
| Time | O(n × m) |
| Space | O(1) |

---

# Better Approach

Since every row is sorted, perform Binary Search on every row.

---

## Code

```cpp
class Solution {
public:

    bool searchMatrix(vector<vector<int>>& matrix, int target) {

        int n = matrix.size();
        int m = matrix[0].size();

        for(int i = 0; i < n; i++) {
            if(binary_search(matrix[i].begin(), matrix[i].end(), target))
                return true;
        }
        return false;
    }
};
```

---

## Complexity

Binary Search on one row:

```cpp
O(log m)
```

Total:

```cpp
O(n log m)
```

---

# Optimal Approach (Staircase Search)

## Key Observation

Every row:

```text
Left → Right
```

is increasing.

Every column:

```text
Top → Bottom
```

is increasing.

---

Choose the:

```text
Top Right Corner
```

or

```text
Bottom Left Corner
```

because from these positions,

one move always eliminates either an entire row or an entire column.

---

# Why Top Right?

Start at:

```text
      ↓
1  4  7 11
2  5  8 12
3  6  9 16
```

Current element:

```cpp
11
```

---

### If

```cpp
target < current
```

Move:

```text
Left
```

Because everything below is even larger.

---

### If

```cpp
target > current
```

Move:

```text
Down
```

Because everything left is smaller.

---

Thus every comparison removes one complete row or column.

---

# Optimal Code

```cpp
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {

        int n = matrix.size();
        int m = matrix[0].size();

        int row = 0;
        int col = m - 1;

        while(row < n && col >= 0) {
            if(matrix[row][col] == target)
                return true;
            else if(matrix[row][col] > target)
                col--;
            else
                row++;
        }
        return false;
    }
};
```

---

# Why Does Staircase Search Work?

At every step, we eliminate either:

```cpp
One complete row
```

or

```cpp
One complete column.
```

Example:

Current =

```cpp
15
```

If:

```cpp
target < 15
```

Everything below is even larger. Entire column can be ignored.

---

If:

```cpp
target > 15
```

Everything left is smaller. Entire row can be ignored. Thus each move reduces the search space.

---

# Complexity Analysis

Worst case:

Move at most:

```cpp
n rows
```

and

```cpp
m columns
```

Total moves:

```cpp
n + m
```

| Complexity | Value |
|------------|--------|
| Time | O(n + m) |
| Space | O(1) |

---

# Pattern Recognition

This problem belongs to:

```text
Matrix Traversal + Greedy Elimination
```

Unlike LC 74, Binary Search on the whole matrix **cannot** be used because the matrix is **not globally sorted**.

The staircase search works because each comparison eliminates an entire row or an entire column, giving an optimal **O(n + m)** solution.