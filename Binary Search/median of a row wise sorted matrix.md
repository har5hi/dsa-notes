# Matrix Median (Every Row is Sorted)

## Problem Statement

Given an `n × m` matrix where:

- Every row is sorted in non-decreasing order.
- Total number of elements is odd.

Return the **median** of the matrix.

> **Note:** The matrix is **not globally sorted**, only each row is sorted.

---

## Example

```cpp
Input:

1  3  5
2  6  9
3  6  9
```

Output:

```cpp
5
```

---

### Explanation

After sorting all elements:

```text
1 2 3 3 5 6 6 9 9
```

Median:

```cpp
5
```

---

# Brute Force Approach

## Idea

Store all elements in a single array.

Sort the array.

Return:

```cpp
arr[(n*m)/2]
```

---

## Code

```cpp
class Solution {
public:

    int median(vector<vector<int>> &matrix, int R, int C) {

        vector<int> temp;

        for(int i = 0; i < R; i++) {
            for(int j = 0; j < C; j++) {
                temp.push_back(matrix[i][j]);
            }
        }

        sort(temp.begin(), temp.end());
        return temp[(R*C)/2];
    }
};
```

---

## Complexity Analysis

Sorting:

```cpp
O((R*C) log(R*C))
```

Space:

```cpp
O(R*C)
```

---

# Better Observation

Rows are already sorted. So we can avoid sorting all elements? Yes
Instead of searching for the median directly, Binary Search on the **value**.

---

# Optimal Approach (Binary Search on Answer)

## Key Observation

Suppose the median is:

```cpp
x
```

Count:

```cpp
How many elements are ≤ x ?
```

If this count is:

```cpp
<= half
```

Need a larger value.

Otherwise,

try a smaller value.

---

This creates a monotonic property.

---

# Search Space

The median must lie between:
Minimum element of the matrix:
```cpp
low
```
and
Maximum element of the matrix:
```cpp
high
```
Since rows are sorted:
Minimum:

```cpp
matrix[i][0]
```

Maximum:

```cpp
matrix[i][C-1]
```

---

## Finding Search Space

```cpp
low = INT_MAX
high = INT_MIN
```

For every row:

```cpp
low = min(low, matrix[i][0]);
high = max(high, matrix[i][C-1]);
```

---

# Important Observation

Suppose:

```cpp
mid = 5
```

Need:

```cpp
How many numbers ≤ 5 ?
```

Every row is sorted.

So instead of scanning each row,

use:

```cpp
upper_bound()
```

---

# Why upper_bound()?

Suppose row is:

```text
1 3 5 7 9
```

Need numbers:

```cpp
<=5
```

```cpp
upper_bound(5)
```

returns index:

```cpp
3
```

Exactly:

```cpp
3 elements
```

are ≤ 5.

---

# Formula

```cpp
count +=

upper_bound(row.begin(), row.end(), mid) - row.begin();
```

---

# Binary Search Logic

Total elements:

```cpp
R*C
```

Median position:

```cpp
required = (R*C)/2
```

---

### If

```cpp
count <= required
```

Median is larger.

```cpp
low = mid + 1
```

---

### Else

Median could be:

```cpp
mid
```

Search left.

```cpp
high = mid - 1
```

---

# Optimal Code

```cpp
class Solution {
public:

    int median(vector<vector<int>> &matrix, int R, int C) {

        int low = INT_MAX;
        int high = INT_MIN;

        for(int i = 0; i < R; i++) {
            low = min(low, matrix[i][0]);
            high = max(high, matrix[i][C-1]);
        }

        int required = (R*C)/2;

        while(low <= high) {

            int mid = low + (high-low)/2;
            int count = 0;

            for(int i = 0; i < R; i++) {

                count += upper_bound(matrix[i].begin(), matrix[i].end(), mid) - matrix[i].begin();
            }

            if(count <= required) {
                low = mid + 1;
            }

            else {
                high = mid - 1;
            }
        }
        return low;
    }
};
```

---

# Why Binary Search on Answer?

We are **not searching an index**. We are searching for the value that satisfies:
```cpp
Number of elements ≤ value
>
(R*C)/2
```
This is the classic Binary Search on Answer pattern.

---

# Complexity Analysis

Let:

```cpp
R = rows
C = columns
```

Binary Search on values:

```cpp
O(log(max-min))
```

Each iteration:
For every row:

```cpp
upper_bound()

O(log C)
```

Total:

```cpp
O(R log C)
```

Overall:

| Complexity | Value |
|------------|--------|
| Time | O(R × log(C) × log(maxValue-minValue)) |
| Space | O(1) |

---