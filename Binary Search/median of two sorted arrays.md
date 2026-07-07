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


# K-th Element of Two Sorted Arrays

## Problem Statement

Given two sorted arrays:

```cpp
arr1[]
arr2[]
```

and an integer:

```cpp
k
```

Return the:

```cpp
k-th smallest element
```

in the combined sorted array.

---

## Example

```cpp
arr1 = [2,3,6,7,9]
arr2 = [1,4,8,10]

k = 5
```

Merged:

```text
1 2 3 4 6 7 8 9 10
```

5th element:

```cpp
6
```

---

# Brute Force Approach

Merge both arrays.

Return:

```cpp
merged[k-1]
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

Traverse merged order without creating a new array.

Stop at:

```cpp
k-th position
```

---

## Complexity

```cpp
Time  : O(k)
Space : O(1)
```

---

# Optimal Approach (Binary Search Partition)

## Core Idea

Exactly same as Median. We partition both arrays such that:

```cpp
left part contains k elements
```

---

# Partition

Suppose:

```cpp
cut1
```

elements are taken from array1.

Then:

```cpp
cut2 = k - cut1
```

must be taken from array2.

---

## Partition Structure

```text
Array1

l1 | r1

Array2

l2 | r2
```

where

```cpp
l1 = arr1[cut1-1]
r1 = arr1[cut1]

l2 = arr2[cut2-1]
r2 = arr2[cut2]
```

---

# Valid Partition Condition

Exactly same as median:

```cpp
l1 <= r2
```

and

```cpp
l2 <= r1
```

---

# Why?

Then all elements in left partition are smaller than all elements in right partition.
Since left partition contains exactly:

```cpp
k elements
```

the k-th element becomes:

```cpp
max(l1,l2)
```

---

# Search Space Derivation

We cannot choose any cut.

Suppose:

```cpp
cut2 = k - cut1
```

must remain valid.

Therefore:

```cpp
0 <= cut2 <= n2
```

---

From:

```cpp
cut2 >= 0
```

```cpp
k - cut1 >= 0
```

```cpp
cut1 <= k
```

---

From:

```cpp
cut2 <= n2
```

```cpp
k - cut1 <= n2
```

```cpp
cut1 >= k - n2
```

---

Therefore:

```cpp
low  = max(0, k-n2)
high = min(k, n1)
```

This is the most important difference from Median.

---

# Binary Search Logic

### If

```cpp
l1 > r2
```

Too many elements taken from array1. Move left.

```cpp
high = cut1 - 1
```

---

### If

```cpp
l2 > r1
```

Too few elements taken from array1. Move right.

```cpp
low = cut1 + 1
```

---

### Else

Valid partition found.

Answer:

```cpp
max(l1,l2)
```

---

# Optimal Code

```cpp
class Solution {
public:

    int kthElement(vector<int>& arr1, vector<int>& arr2, int n1, int n2, int k) {

        if(n1 > n2) return kthElement( arr2, arr1, n2, n1, k);

        int low = max(0, k - n2);
        int high = min(k, n1);

        while(low <= high) {

            int cut1 = (low + high) / 2;
            int cut2 = k - cut1;

            int l1 = INT_MIN;
            int l2 = INT_MIN;
            int r1 = INT_MAX;
            int r2 = INT_MAX;

            if(cut1 > 0) l1 = arr1[cut1 - 1];
            if(cut2 > 0) l2 = arr2[cut2 - 1];
            if(cut1 < n1) r1 = arr1[cut1];
            if(cut2 < n2) r2 = arr2[cut2];

            if(l1 <= r2 && l2 <= r1) {
                return max(l1, l2);
            }
            else if(l1 > r2) {
                high = cut1 - 1;
            }
            else {
                low = cut1 + 1;
            }
        }
        return 0;
    }
};
```

---

# Dry Run

```cpp
arr1 = [2,3,6,7,9]
arr2 = [1,4,8,10]

k = 5
```

---

### Iteration 1

```cpp
cut1 = 2
cut2 = 3
```

Partition:

```text
2 3 | 6 7 9
1 4 8 | 10
```

```cpp
l1 = 3
r1 = 6

l2 = 8
r2 = 10
```

```cpp
l2 > r1
```

Move right.

---

### Iteration 2

```cpp
cut1 = 3
cut2 = 2
```

Partition:

```text
2 3 6 | 7 9
1 4 | 8 10
```

```cpp
l1 = 6
r1 = 7

l2 = 4
r2 = 8
```

Valid.

Answer:

```cpp
max(6,4)
=
6
```

---

# Complexity Analysis

| Complexity | Value |
|------------|--------|
| Time | O(log(min(n1,n2))) |
| Space | O(1) |

---

# Interview Trick

If you know **Median of Two Sorted Arrays**, then **K-th Element of Two Sorted Arrays** is the exact same problem.

Just replace:

```cpp
left = (n1+n2+1)/2
```

with

```cpp
left = k
```

and return:

```cpp
max(l1,l2)
```

instead of calculating the median.