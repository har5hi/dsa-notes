# 4. Median of Two Sorted Arrays

## Problem Statement

Given two sorted arrays:

```cpp
nums1
nums2
```

Return the median of the combined sorted array.

The overall run time complexity must be:

```cpp
O(log(m+n))
```

---

## Example 1

```cpp
nums1 = [1,3]
nums2 = [2]
```

Merged:

```cpp
[1,2,3]
```

Median:

```cpp
2
```

---

## Example 2

```cpp
nums1 = [1,2]
nums2 = [3,4]
```

Merged:

```cpp
[1,2,3,4]
```

Median:

```cpp
(2+3)/2 = 2.5
```

---

# Brute Force Approach

## Idea

Merge both arrays.

Then find median.

---

## Code

```cpp
vector<int> temp;

merge(nums1.begin(), nums1.end(), nums2.begin(), nums2.end(), back_inserter(temp));
```

Then:

### Odd Length

```cpp
temp[n/2]
```

### Even Length

```cpp
(temp[n/2] + temp[n/2 - 1]) / 2.0
```

---

## Complexity

```cpp
Time  : O(n1+n2)
Space : O(n1+n2)
```

---

# Better Approach

Use two pointers.

Find only the median positions while traversing.

No extra array needed.

---

## Complexity

```cpp
Time  : O(n1+n2)
Space : O(1)
```

Still not acceptable because question asks:

```cpp
O(log(m+n))
```

---

# Optimal Approach (Binary Search Partition)

## Core Idea

Instead of finding the median directly, find a partition such that:

```text
Left Half | Right Half
```

contains:

```cpp
(n1+n2+1)/2 elements
```
on the left side.

---

# Example

```cpp
nums1 = [1,3,8]
nums2 = [7,9,10,11]
```

Possible partition:

```text
1 3 | 8
7 9 | 10 11
```

Left side:

```text
1 3 7 9
```

Right side:

```text
8 10 11
```

Not valid.

---

We need:

```cpp
max(left part)
<=
min(right part)
```

---

# Important Observation

For a valid partition:

```cpp
l1 <= r2
```

and

```cpp
l2 <= r1
```

where:

```text
l1 = last element of left half in nums1
r1 = first element of right half in nums1

l2 = last element of left half in nums2
r2 = first element of right half in nums2
```

---

# Why Binary Search?

Suppose:

```cpp
l1 > r2
```

Then:

```text
Too many elements taken from nums1
```

Move left.

```cpp
high = mid1 - 1
```

---

Suppose:

```cpp
l2 > r1
```

Then:

```text
Too few elements taken from nums1
```

Move right.

```cpp
low = mid1 + 1
```

---

# Why Binary Search on Smaller Array?

```cpp
if(n1 > n2) - swap
```

We always binary search on the smaller array.

This keeps complexity:

```cpp
O(log(min(n1,n2)))
```

instead of:

```cpp
O(log(max(n1,n2)))
```

and prevents invalid partitions.

---

# Partition Formula

Total elements:

```cpp
n = n1 + n2
```

Left half size:

```cpp
left = (n1+n2+1)/2
```

If:

```cpp
mid1
```

elements are taken from nums1,

then:

```cpp
mid2 = left - mid1
```

must be taken from nums2.

---

# Why +1 ?

```cpp
left = (n1+n2+1)/2
```

handles both odd and even lengths.

---

### Odd Length

```cpp
n = 7
```

```cpp
(7+1)/2 = 4
```

Left side gets:

```cpp
4 elements
```

Median becomes:

```cpp
max(l1,l2)
```

---

### Even Length

```cpp
n = 8
```

```cpp
(8+1)/2 = 4
```

Left side gets:

```cpp
4 elements
```

Median:

```cpp
(max(l1,l2)+min(r1,r2))/2
```

---

# Optimal Code

```cpp
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {

        int n1 = nums1.size();
        int n2 = nums2.size();

        if(n1 > n2)
            return findMedianSortedArrays(nums2,nums1);

        int low = 0;
        int high = n1;

        int left = (n1+n2+1)/2;

        int n = n1+n2;

        while(low <= high){

            int mid1 = (low+high)>>1;

            int mid2 = left-mid1;

            int l1 = INT_MIN;
            int l2 = INT_MIN;
            int r1 = INT_MAX;
            int r2 = INT_MAX;

            if(mid1 < n1)
                r1 = nums1[mid1];

            if(mid2 < n2)
                r2 = nums2[mid2];

            if(mid1-1 >= 0)
                l1 = nums1[mid1-1];

            if(mid2-1 >= 0)
                l2 = nums2[mid2-1];

            if(l1 <= r2 && l2 <= r1){

                if(n%2==1)
                    return max(l1,l2);

                return
                (double)(max(l1,l2) + min(r1,r2))/2.0;
            }

            else if(l1 > r2){
                high = mid1-1;
            }

            else{
                low = mid1+1;
            }
        }
        return 0;
    }
};
```

---

# Dry Run

```cpp
nums1 = [1,3]
nums2 = [2]
```

After swap:

```cpp
nums1 = [2]
nums2 = [1,3]
```

---

### Iteration 1

```cpp
mid1 = 0
mid2 = 2
```

```text
| 2
1 3 |
```

```cpp
l1 = -∞
r1 = 2

l2 = 3
r2 = +∞
```

Not valid.

```cpp
l2 > r1
```

Move right.

---

### Iteration 2

```cpp
mid1 = 1
mid2 = 1
```

```text
2 |
1 | 3
```

```cpp
l1 = 2
r1 = +∞

l2 = 1
r2 = 3
```

Valid.

Odd length:

```cpp
median = max(2,1)
       = 2
```

---

# Complexity Analysis

| Complexity | Value |
|------------|--------|
| Time | O(log(min(n1,n2))) |
| Space | O(1) |

---

# Interview One-Liner

The idea is to binary search the smaller array and find a partition such that all elements on the left side are less than or equal to all elements on the right side. Once the correct partition is found, the median can be directly computed from the boundary elements.