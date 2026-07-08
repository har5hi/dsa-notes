# Row with Maximum Number of 1's

## Problem Statement

Given a binary matrix of size:

```cpp
n × m
```

where each row is sorted (all `0`s appear before `1`s),

find the **index of the row having the maximum number of 1's**.
If no row contains `1`, return:

```cpp
-1
```

---

## Example

```cpp
Matrix =

0 0 1 1
0 1 1 1
0 0 0 1
0 0 0 0
```

Output:

```cpp
1
```

# Brute Force Approach

## Idea

Traverse every row.

Count the number of 1's.

Store:

- maximum count
- corresponding row index

---

## Code

```cpp
class Solution {
public:

    int rowWithMax1s(vector<vector<int>>& mat) {

        int n = mat.size();
        int m = mat[0].size();

        int maxCount = 0;
        int index = -1;

        for(int i = 0; i < n; i++) {

            int cnt = 0;

            for(int j = 0; j < m; j++) {

                if(mat[i][j] == 1)
                    cnt++;
            }

            if(cnt > maxCount) {
                maxCount = cnt;
                index = i;
            }
        }

        return index;
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

# Better Observation

Each row is already sorted.

Example:

```text
0 0 0 1 1 1
```

Instead of counting every element,

find the **first occurrence of 1**.

Then:

```cpp
Number of 1's = m - firstOneIndex
```

---

# How to Find First 1?

Use Binary Search.

---

## Example

```text
0 0 0 1 1 1
```

First `1` occurs at:

```cpp
index = 3
```

Number of 1's:

```cpp
6 - 3 = 3
```

---

# Optimal Approach

For every row:

1. Binary Search first `1`.
2. Count:

```cpp
m - firstOne
```

3. Update maximum.

---

# Why Does It Work?

Since rows are sorted:

```text
0 0 0 1 1 1
```

Once first `1` is found,

everything after it is also `1`.

Therefore:

```cpp
count = m - index
```

---

# Optimal Code

```cpp
class Solution {
public:

    int lowerBound(vector<int>& arr, int m) {

        int low = 0;
        int high = m - 1;

        int ans = m;

        while(low <= high) {

            int mid = low + (high - low) / 2;

            if(arr[mid] == 1) {
                ans = mid;
                high = mid - 1;
            }
            else {
                low = mid + 1;
            }
        }
        return ans;
    }

    int rowWithMax1s(vector<vector<int>>& mat) {

        int n = mat.size();
        int m = mat[0].size();

        int index = -1;
        int maxCount = 0;

        for(int i = 0; i < n; i++) {

            int firstOne = lowerBound(mat[i], m);
            int cntOnes = m - firstOne;

            if(cntOnes > maxCount) {
                maxCount = cntOnes;
                index = i;
            }
        }
        return index;
    }
};
```

---

# Why Return `m` in Lower Bound?

Suppose row is:

```text
0 0 0 0
```

No `1` exists.

Lower Bound returns:

```cpp
m
```

Then:

```cpp
count = m - m = 0
```

Works automatically.

---

# Complexity Analysis

### Binary Search

```cpp
O(log m)
```

for one row.

### Total

```cpp
n rows
```

Overall:

| Complexity | Value |
|------------|--------|
| Time | O(n × log m) |
| Space | O(1) |