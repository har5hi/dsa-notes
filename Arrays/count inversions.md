# Count Inversions

## Problem Statement

Given an array `arr[]`, count the number of inversions.

An inversion is a pair `(i, j)` such that:

- `i < j`
- `arr[i] > arr[j]`

### Example

Input:
arr = [5, 3, 2, 4, 1]

Output:
Inversions are:

(5,3)(5,2)(5,4)(5,1)(3,2)(3,1)(2,1)(4,1)
Total = 8

# Brute Force Approach

## Idea

Check every possible pair `(i, j)`.

If:

arr[i] > arr[j]

then it forms an inversion.

Count all such pairs.

## Algorithm

For every element:

1. Fix `i`
2. Check all elements after it (`j > i`)
3. If `arr[i] > arr[j]`
   increment count

## Code

```cpp
long long inversionCount(vector<int>& arr) {
    long long cnt = 0;
    int n = arr.size();

    for(int i = 0; i < n; i++) {
        for(int j = i + 1; j < n; j++) {
            if(arr[i] > arr[j]) {
                cnt++;
            }
        }
    }

    return cnt;
}
```

## Complexity

Time: O(N²)

Space: O(1)

## Why Not Optimal?

For every element we compare with all remaining elements.

For N = 100000:

O(N²) ≈ 10¹⁰ operations, which is too slow.

# Optimal Approach (Merge Sort)

## Key Observation

During Merge Sort:

Left half and right half are already sorted.

Suppose during merge:

Left:
[3, 5, 7]

Right:
[1, 2, 6]

If:

3 > 1

then:

5 > 1
7 > 1

also true because left half is sorted.

So instead of counting one-by-one, we can count many inversions together.

# Why Merge Sort Works?

Consider:

Left = [3, 5, 7]
Right = [1, 2, 6]

Pointers:

i -> Left
j -> Right

Current comparison:

3 > 1

Since Left array is sorted:

3, 5, 7

all elements from index i till end are greater than 1.

Number of inversions:

= remaining elements in Left
= (mid - i + 1)

Here:

= 3

Inversions:

(3,1)
(5,1)
(7,1)

Count them all at once.

This is what makes Merge Sort efficient.

# Merge Sort Code

```cpp
class Solution {
private:

    long long merge(vector<int>& arr, int low, int mid, int high) {

        vector<int> temp;

        int left = low;
        int right = mid + 1;

        long long cnt = 0;

        while(left <= mid && right <= high) {

            if(arr[left] <= arr[right]) {
                temp.push_back(arr[left]);
                left++;
            }
            else {

                // inversion found
                cnt += (mid - left + 1);

                temp.push_back(arr[right]);
                right++;
            }
        }

        while(left <= mid) {
            temp.push_back(arr[left]);
            left++;
        }

        while(right <= high) {
            temp.push_back(arr[right]);
            right++;
        }

        for(int i = low; i <= high; i++) {
            arr[i] = temp[i - low];
        }

        return cnt;
    }

    long long mergeSort(vector<int>& arr, int low, int high) {

        if(low >= high) return 0;

        int mid = (low + high) / 2;

        long long cnt = 0;

        cnt += mergeSort(arr, low, mid);

        cnt += mergeSort(arr, mid + 1, high);

        cnt += merge(arr, low, mid, high);

        return cnt;
    }

public:

    long long inversionCount(vector<int>& arr) {

        return mergeSort(arr, 0, arr.size() - 1);
    }
};
```

# Understanding the Important Line

```cpp
cnt += (mid - left + 1);
```

Why?

Current situation:

Left:
[3,5,7]

Right:
[1]

Pointer:

left -> 3

Since:

3 > 1

and Left array is sorted,

all remaining elements are also greater than 1:

3,5,7

Number of such elements:

= mid - left + 1

Therefore:

cnt += (mid - left + 1)

# Complexity Analysis

## Time Complexity

Merge Sort depth = log N

Each level processes N elements.

Time: O(N log N)

## Space Complexity

Temporary array used during merge.

Space: O(N)

