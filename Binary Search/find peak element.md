# 162. Find Peak Element

## Problem Statement

A **peak element** is an element that is strictly greater than its neighbors.

Given an integer array `nums`, find a peak element and return its index.

You may assume:

```cpp
nums[-1] = nums[n] = -∞
```

If multiple peaks exist, return the index of any peak.

---

# Brute Force Approach

## Idea

Traverse the array and check every element.

If an element is greater than both neighbors, it is a peak.

---

## Code

```cpp
class Solution {
public:
    int findPeakElement(vector<int>& nums) {

        int n = nums.size();

        for(int i = 0; i < n; i++) {

            bool left =
                (i == 0 || nums[i] > nums[i - 1]);

            bool right =
                (i == n - 1 || nums[i] > nums[i + 1]);

            if(left && right)
                return i;
        }

        return -1;
    }
};
```

---

## Complexity Analysis

| Complexity | Value |
|------------|--------|
| Time | O(n) |
| Space | O(1) |

---

# Optimal Approach (Binary Search)

## Key Observation

We are **not asked to find the maximum element**.

We only need **any peak element**.

---

A peak always exists because:

```cpp
nums[-1] = nums[n] = -∞
```

---

# Important Observation

At any index:

### Case 1: Increasing Slope

```text
1 2 3 4 5
      ^
     mid
```

If:

```cpp
nums[mid] < nums[mid + 1]
```

Then a peak must exist on the right side.

Why?

Because:

- Array keeps increasing → last element becomes peak.
- Or it increases and then decreases → peak formed somewhere.

So:

```cpp
low = mid + 1;
```

---

### Case 2: Decreasing Slope

```text
5 4 3 2 1
    ^
   mid
```

If:

```cpp
nums[mid] > nums[mid + 1]
```

Then a peak exists on the left side (including mid).

Why?

Because:

- We are already on a descending slope.
- A peak must exist before or at mid.

So:

```cpp
high = mid;
```

---

# Why Binary Search Works

Instead of checking whether `mid` is peak directly,

we check the direction:

```cpp
nums[mid] < nums[mid + 1]
```

Move right.

Else:

```cpp
move left.
```

Eventually both pointers meet at a peak.

---

# Optimal Code

```cpp
class Solution {
public:
    int findPeakElement(vector<int>& nums) {

        int low = 0;
        int high = nums.size() - 1;

        while(low < high) {

            int mid = low + (high - low) / 2;

            if(nums[mid] < nums[mid + 1]) {
                low = mid + 1;
            }
            else {
                high = mid;
            }
        }

        return low;
    }
};
```

---

# Complexity Analysis

| Complexity | Value |
|------------|--------|
| Time | O(log n) |
| Space | O(1) |

---

# Key Takeaway

For LC 162, don't search for the maximum element.

Search for the **direction of the slope**:

```cpp
Increasing  -> go right
Decreasing  -> go left
```

This guarantees reaching a peak in **O(log n)** time.

---

# 1901. Find a Peak Element II

## Problem Statement

A **peak element** in a 2D matrix is an element that is **strictly greater than its four neighbors**:

- Up
- Down
- Left
- Right

(Cells outside the matrix are considered as `-∞`.)

Return the position:

```cpp
[row, col]
```

of **any peak element**.

---

## Example

```cpp
Input:

mat =

10 20 15
21 30 14
 7 16 32
```

Output:

```cpp
[1,1]
```

or

```cpp
[2,2]
```

Both are valid peaks.

---

# Brute Force Approach

## Idea

For every cell: Check all four neighbours.

If current element is greater than:

- Up
- Down
- Left
- Right

then it is a peak.

Return its coordinates.

---

## Code

```cpp
class Solution {
public:

    vector<int> findPeakGrid(vector<vector<int>>& mat) {

        int n = mat.size();
        int m = mat[0].size();

        for(int i = 0; i < n; i++) {
            for(int j = 0; j < m; j++) {

                bool up = (i == 0 || mat[i][j] > mat[i-1][j]);
                bool down = (i == n-1 || mat[i][j] > mat[i+1][j]);
                bool left = (j == 0 || mat[i][j] > mat[i][j-1]);
                bool right = (j == m-1 || mat[i][j] > mat[i][j+1]);

                if(up && down && left && right)
                    return {i,j};
            }
        }
        return {-1,-1};
    }
};
```

---

# Complexity Analysis

| Complexity | Value |
|------------|--------|
| Time | O(n × m) |
| Space | O(1) |

---

# Optimal Approach (Binary Search on Columns)

## Key Observation

This problem is an extension of:

```text
LC 162 - Find Peak Element
```

Instead of Binary Searching on indices, we Binary Search on **columns**.

---

# Main Idea

Suppose we choose:

```cpp
mid column
```

Find:

```cpp
largest element
```

in that column.

Why?

Because if a peak exists in this column, it must be the maximum element of that column.
No element above or below it can be larger.
Now only compare:

```text
Left neighbour

Right neighbour
```

---

# Binary Search Logic

Suppose:

```cpp
current = mat[row][mid]
```

---

### Case 1

If:

```cpp
current > left

AND

current > right
```

Current is a peak.

Return.

---

### Case 2

If:

```cpp
left > current
```

Peak must exist on the left.

Search:

```cpp
high = mid - 1
```

---

### Case 3

Otherwise:

```cpp
right > current
```

Peak must exist on the right.

Search:

```cpp
low = mid + 1
```

---

# Why Does This Work?

This is exactly like LC 162. If one neighbour is larger, moving toward that neighbour guarantees reaching a peak.
Binary Search safely discards half the columns.

---

# Optimal Code

```cpp
class Solution {
public:

    int maxElement(vector<vector<int>>& mat, int col) {

        int n = mat.size();
        int row = 0;

        for(int i = 1; i < n; i++) {

            if(mat[i][col] > mat[row][col])
                row = i;
        }
        return row;
    }

    vector<int> findPeakGrid(vector<vector<int>>& mat) {

        int n = mat.size();
        int m = mat[0].size();

        int low = 0;
        int high = m - 1;

        while(low <= high) {

            int mid = low + (high-low)/2;

            int row = maxElement(mat, mid);

            int left = -1;
            int right = -1;

            if(mid-1 >= 0) 
                left = mat[row][mid-1];

            if(mid+1 < m)
                right = mat[row][mid+1];

            if(mat[row][mid] > left && mat[row][mid] > right) {
                return {row, mid};
            }

            else if(left > mat[row][mid]) {
                high = mid - 1;
            }

            else {
                low = mid + 1;
            }
        }
        return {-1,-1};
    }
};
```

---

# Why Is It Correct?

We always select the largest element in the current column.

Therefore,

```cpp
Up
```

and

```cpp
Down
```

cannot be larger.

Only:

```cpp
Left

Right
```

need checking.

If one side is larger, moving towards it guarantees a peak.
Exactly the same idea as LC 162.

---

# Complexity Analysis

Finding maximum in one column:

```cpp
O(n)
```

Binary Search over columns:

```cpp
O(log m)
```

Total:

| Complexity | Value |
|------------|--------|
| Time | O(n × log m) |
| Space | O(1) |

---