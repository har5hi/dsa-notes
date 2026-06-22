# LeetCode 88 - Merge Sorted Array
---

# Problem Statement

You are given two sorted integer arrays:

```cpp
nums1
```

and

```cpp
nums2
```

with sizes:

```cpp
m and n
```

respectively.

`nums1` has a size of:

```cpp
m + n
```

where the last `n` elements are `0` and are meant to hold elements from `nums2`.

Merge `nums2` into `nums1` as one sorted array.

The final sorted array should be stored inside `nums1`.

---

## Example

### Input

```cpp
nums1 = [1,2,3,0,0,0]
m = 3

nums2 = [2,5,6]
n = 3
```

### Output

```cpp
[1,2,2,3,5,6]
```

---

# Brute Force Approach

## Idea

1. Copy all elements of `nums2` into the empty spaces of `nums1`.
2. Sort the entire array.

---

## Algorithm

1. Start inserting elements of `nums2` from index `m`.
2. Fill remaining positions.
3. Sort complete `nums1`.

---

## Code

```cpp
class Solution {
public:
    void merge(vector<int>& nums1, int m,
               vector<int>& nums2, int n) {

        for(int i = 0; i < n; i++) {
            nums1[m + i] = nums2[i];
        }

        sort(nums1.begin(), nums1.end());
    }
};
```

---

## Dry Run

### Input

```cpp
nums1 = [1,2,3,0,0,0]
nums2 = [2,5,6]
```

Copy nums2:

```cpp
nums1 = [1,2,3,2,5,6]
```

Sort:

```cpp
nums1 = [1,2,2,3,5,6]
```

---

## Complexity Analysis

### Time Complexity

Copying:

```cpp
O(N)
```

Sorting:

```cpp
O((M+N) log(M+N))
```

Overall:

```cpp
O((M+N) log(M+N))
```

### Space Complexity

```cpp
O(1)
```

(ignoring sorting internals)

---

# Better Approach (Extra Array)

## Idea

Since both arrays are already sorted:

Use two pointers and merge them exactly like Merge Sort.

Store result in a temporary array.

---

## Algorithm

1. Take pointers:
   ```cpp
   i = 0
   j = 0
   ```

2. Compare elements.
3. Push smaller element into temp.
4. Move corresponding pointer.
5. Add remaining elements.
6. Copy temp back to nums1.

---

## Code

```cpp
class Solution {
public:
    void merge(vector<int>& nums1, int m,
               vector<int>& nums2, int n) {

        vector<int> temp;

        int i = 0;
        int j = 0;

        while(i < m && j < n) {

            if(nums1[i] <= nums2[j]) {
                temp.push_back(nums1[i++]);
            }
            else {
                temp.push_back(nums2[j++]);
            }
        }

        while(i < m)
            temp.push_back(nums1[i++]);

        while(j < n)
            temp.push_back(nums2[j++]);

        for(int k = 0; k < m + n; k++)
            nums1[k] = temp[k];
    }
};
```

---

## Complexity

### Time

```cpp
O(M + N)
```

### Space

```cpp
O(M + N)
```

---

# Optimal Approach (Three Pointers)

## Observation

`nums1` already has:

```cpp
n
```

empty spaces at the end.

Instead of merging from the front:

👉 Merge from the back.

This avoids shifting elements.

---

## Key Idea

Use three pointers:

```cpp
i = m - 1      // last valid element of nums1
j = n - 1      // last element of nums2
k = m + n - 1  // last position of nums1
```

Always place the larger element at position `k`.

---

## Why Backward Merging?

Suppose:

```cpp
nums1 = [1,2,3,0,0,0]
nums2 = [2,5,6]
```

If we start from front:

```cpp
1,2,3
```

may need shifting.

If we start from back:

```cpp
6 → last position
5 → second last
3 → next
```

No shifting required.

---

## Algorithm

1. Set:
   ```cpp
   i = m - 1
   j = n - 1
   k = m + n - 1
   ```

2. While both arrays have elements:
   - Compare `nums1[i]` and `nums2[j]`
   - Place larger value at `nums1[k]`

3. If elements remain in nums2:
   - Copy them.

4. Return.

---

## Optimal Code

```cpp
class Solution {
public:
    void merge(vector<int>& nums1, int m,
               vector<int>& nums2, int n) {

        int i = m - 1;
        int j = n - 1;
        int k = m + n - 1;

        while(i >= 0 && j >= 0) {

            if(nums1[i] > nums2[j]) {
                nums1[k] = nums1[i];
                i--;
            }
            else {
                nums1[k] = nums2[j];
                j--;
            }

            k--;
        }

        while(j >= 0) {
            nums1[k] = nums2[j];
            j--;
            k--;
        }
    }
};
```

---

# Detailed Dry Run

### Input

```cpp
nums1 = [1,2,3,0,0,0]
m = 3

nums2 = [2,5,6]
n = 3
```

---

### Initial State

```cpp
i = 2 → 3
j = 2 → 6
k = 5
```

Array:

```cpp
[1,2,3,0,0,0]
```

---

### Iteration 1

Compare:

```cpp
3 vs 6
```

Place 6:

```cpp
[1,2,3,0,0,6]
```

Update:

```cpp
j = 1
k = 4
```

---

### Iteration 2

Compare:

```cpp
3 vs 5
```

Place 5:

```cpp
[1,2,3,0,5,6]
```

Update:

```cpp
j = 0
k = 3
```

---

### Iteration 3

Compare:

```cpp
3 vs 2
```

Place 3:

```cpp
[1,2,3,3,5,6]
```

Update:

```cpp
i = 1
k = 2
```

---

### Iteration 4

Compare:

```cpp
2 vs 2
```

Place nums2 value:

```cpp
[1,2,2,3,5,6]
```

Update:

```cpp
j = -1
```

Stop.

---

### Final Answer

```cpp
[1,2,2,3,5,6]
```

---

# Important Edge Cases

### Case 1

```cpp
nums1 = [0]
m = 0

nums2 = [1]
n = 1
```

Output:

```cpp
[1]
```

---

### Case 2

```cpp
nums1 = [1]
m = 1

nums2 = []
n = 0
```

Output:

```cpp
[1]
```

---

### Case 3

```cpp
nums1 = [2,0]
m = 1

nums2 = [1]
n = 1
```

Output:

```cpp
[1,2]
```

---

# Interview Explanation (30 Seconds)

Since both arrays are already sorted, we can merge them like Merge Sort. To achieve O(1) extra space, we start filling `nums1` from the back. We use three pointers:

- `i` → last valid element in nums1
- `j` → last element in nums2
- `k` → last position in nums1

At each step, place the larger element at position `k` and move pointers accordingly. This avoids shifting elements and gives an optimal solution.

---

# Gap Method

> ⚠️ Note:
>
> The Gap Method is **not required for LeetCode 88** because the optimal three-pointer solution already achieves:
>
> ```cpp
> Time  : O(M + N)
> Space : O(1)
> ```
>
> However, Gap Method is an important interview technique and is commonly used in:
>
> - GFG: Merge Two Sorted Arrays Without Extra Space
> - Merge arrays without using extra memory
> - Shell Sort based optimization

---

# Idea

Instead of using an extra array, compare elements that are a certain distance apart (called a gap).

Gradually reduce the gap until it becomes 1.

This is inspired by **Shell Sort**.

---

# Gap Formula

After every pass:

```cpp
gap = ceil(gap / 2)
```

Equivalent code:

```cpp
gap = (gap / 2) + (gap % 2);
```

---

# Example

## Input

```cpp
nums1 = [1,4,8,10]
nums2 = [2,3,9]
```

Combined Virtual Array:

```cpp
[1,4,8,10,2,3,9]
```

Length:

```cpp
7
```

Initial Gap:

```cpp
ceil(7/2) = 4
```

---

# Observation

Treat both arrays as one continuous array.

Indexes:

```cpp
0 1 2 3 | 4 5 6
1 4 8 10| 2 3 9
```

Whenever:

```cpp
left > right
```

swap them.

---

# Helper Function

To access elements:

### If index belongs to nums1

```cpp
index < n
```

Access:

```cpp
nums1[index]
```

### If index belongs to nums2

```cpp
index >= n
```

Access:

```cpp
nums2[index - n]
```

---

# Algorithm

1. Compute total length:

```cpp
len = n + m
```

2. Calculate initial gap:

```cpp
gap = ceil(len/2)
```

3. Compare elements separated by gap.

4. Swap if left > right.

5. Reduce gap.

6. Continue until gap becomes 0.

---

# Code

```cpp
class Solution {
public:

    void swapIfGreater(vector<int>& arr1,
                       vector<int>& arr2,
                       int ind1,
                       int ind2) {

        if(arr1[ind1] > arr2[ind2]) {
            swap(arr1[ind1], arr2[ind2]);
        }
    }

    void merge(vector<int>& nums1,
               int n,
               vector<int>& nums2,
               int m) {

        int len = n + m;

        int gap = (len / 2) + (len % 2);

        while(gap > 0) {

            int left = 0;
            int right = left + gap;

            while(right < len) {

                // both in nums1
                if(left < n && right < n) {

                    swapIfGreater(nums1,
                                  nums1,
                                  left,
                                  right);
                }

                // left in nums1, right in nums2
                else if(left < n && right >= n) {

                    swapIfGreater(nums1,
                                  nums2,
                                  left,
                                  right - n);
                }

                // both in nums2
                else {

                    swapIfGreater(nums2,
                                  nums2,
                                  left - n,
                                  right - n);
                }

                left++;
                right++;
            }

            if(gap == 1)
                break;

            gap = (gap / 2) + (gap % 2);
        }
    }
};
```

---

# Dry Run

## Input

```cpp
nums1 = [1,4,8,10]
nums2 = [2,3,9]
```

Combined View:

```cpp
[1,4,8,10,2,3,9]
```

---

## Gap = 4

Compare:

```cpp
1 vs 2
```

No swap.

Compare:

```cpp
4 vs 3
```

Swap.

Array becomes:

```cpp
[1,3,8,10,2,4,9]
```

Compare:

```cpp
8 vs 9
```

No swap.

---

## Gap = 2

Compare elements 2 apart.

Swap where needed.

Array becomes:

```cpp
[1,2,3,4,8,9,10]
```

---

## Gap = 1

Final pass ensures complete ordering.

Result:

```cpp
nums1 = [1,2,3,4]
nums2 = [8,9,10]
```

Combined:

```cpp
[1,2,3,4,8,9,10]
```

---

# Why It Works?

Each pass:

```cpp
gap = large
```

moves large elements toward the right and smaller elements toward the left.

As gap decreases:

```cpp
4 → 2 → 1
```

the array gradually becomes sorted.

The final gap = 1 pass behaves like a normal adjacent comparison pass.

---

# Complexity Analysis

Let:

```cpp
N = size(nums1)
M = size(nums2)
```

Total:

```cpp
L = N + M
```

### Time Complexity

Number of gap passes:

```cpp
log(L)
```

Each pass:

```cpp
O(L)
```

Total:

```cpp
O(L log L)
```

or

```cpp
O((N + M) log(N + M))
```

---

### Space Complexity

```cpp
O(1)
```

No extra array used.

---

# Comparison of All Approaches

| Approach | Time | Space |
|-----------|---------|---------|
| Copy + Sort | O((N+M)log(N+M)) | O(1) |
| Merge using Temp Array | O(N+M) | O(N+M) |
| Gap Method | O((N+M)log(N+M)) | O(1) |
| Three Pointer (LC 88 Optimal) | O(N+M) | O(1) |

---