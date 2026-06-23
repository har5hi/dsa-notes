# LeetCode 493 - Reverse Pairs

## Problem Statement

Given an integer array nums, return the number of reverse pairs.

A reverse pair is a pair (i, j) such that:
- i < j
- nums[i] > 2 * nums[j]

### Example

Input:
nums = [1,3,2,3,1]

Output: Reverse pairs:
(3,1)(3,1)
Answer = 2

# Brute Force Approach

## Idea

Check every possible pair (i, j).

If:

nums[i] > 2 * nums[j]

then it is a reverse pair.

Count all such pairs.

## Algorithm

1. Fix i
2. Traverse all j > i
3. If nums[i] > 2 * nums[j]
   increment count
4. Return count

## Complexity

Time: O(N²)

Space: O(1)

Not suitable for N = 50000.

# Optimal Approach (Merge Sort)

## Observation
This problem looks similar to Count Inversions.

In Count Inversions: arr[i] > arr[j]
In Reverse Pairs: arr[i] > 2 * arr[j]

The condition changes, but the Merge Sort idea remains the same.

# Key Idea

During Merge Sort: Left half is sorted, Right half is sorted.

Suppose:

Left = [4, 5, 8]
Right = [1, 2, 3]

Check: 4 > 2 × 1 -> True

Since Left is sorted: 5 > 2 × 1, 8 > 2 × 1 will also be true.

Therefore we can count multiple reverse pairs at once.

# Why Two Pointers Work?

Suppose:

Left: [4,5,8]

Right: [1,2,3]

i -> left half
j -> right half

For i = 4: 4 > 2 × 1
True

Count one pair.

Move j.

For i = 5:

5 > 2 × 1
5 > 2 × 2
True

Count accordingly.

Because both halves are sorted, j never moves backward.

This gives O(N) counting during each merge step.\

# Important Difference from Count Inversions

Count Inversions: Count while merging.

Reverse Pairs: Cannot directly count during merging.

Reason: Condition is: nums[i] > 2 * nums[j]

not

nums[i] > nums[j]

So first count valid pairs using two pointers,
then perform the normal merge.

# Counting Function

For every element in left half: Find how many elements in right half satisfy: nums[i] > 2 * nums[j]

Since right half is sorted: Once condition fails, it will fail for all larger elements.

Hence j moves only forward.

Total counting complexity = O(N)

int countPairs(vector<int>& nums, int low, int mid, int high) {

    int right = mid + 1;
    int cnt = 0;

    for(int i = low; i <= mid; i++) {

        while(right <= high &&
              (long long)nums[i] > 2LL * nums[right]) {

            right++;
        }

        cnt += (right - (mid + 1));
    }

    return cnt;
}

# Understanding This Line

cnt += (right - (mid + 1));

Why? Suppose:

Left: [5]

Right: [1,2,3]

Check:

5 > 2×1 ✓
5 > 2×2 ✓
5 > 2×3 ✗

Pointer stops at index of 3.

Valid elements: 1 and 2

Count: right - (mid + 1) = 2

Therefore add 2 reverse pairs.

# Merge Function

Same as normal Merge Sort.

void merge(vector<int>& nums, int low, int mid, int high) {

    vector<int> temp;

    int left = low;
    int right = mid + 1;

    while(left <= mid && right <= high) {

        if(nums[left] <= nums[right]) {
            temp.push_back(nums[left++]);
        }
        else {
            temp.push_back(nums[right++]);
        }
    }

    while(left <= mid) {
        temp.push_back(nums[left++]);
    }

    while(right <= high) {
        temp.push_back(nums[right++]);
    }

    for(int i = low; i <= high; i++) {
        nums[i] = temp[i - low];
    }
}

# Complete Optimal Solution
```cpp
class Solution {
public:

    int countPairs(vector<int>& nums,
                   int low,
                   int mid,
                   int high) {

        int right = mid + 1;
        int cnt = 0;

        for(int i = low; i <= mid; i++) {

            while(right <= high &&
                 (long long)nums[i] > 2LL * nums[right]) {

                right++;
            }

            cnt += right - (mid + 1);
        }

        return cnt;
    }

    void merge(vector<int>& nums,
               int low,
               int mid,
               int high) {

        vector<int> temp;

        int left = low;
        int right = mid + 1;

        while(left <= mid && right <= high) {

            if(nums[left] <= nums[right]) {
                temp.push_back(nums[left++]);
            }
            else {
                temp.push_back(nums[right++]);
            }
        }

        while(left <= mid)
            temp.push_back(nums[left++]);

        while(right <= high)
            temp.push_back(nums[right++]);

        for(int i = low; i <= high; i++) {
            nums[i] = temp[i - low];
        }
    }

    int mergeSort(vector<int>& nums,
                  int low,
                  int high) {

        if(low >= high)
            return 0;

        int mid = (low + high) / 2;

        int cnt = 0;

        cnt += mergeSort(nums, low, mid);

        cnt += mergeSort(nums, mid + 1, high);

        cnt += countPairs(nums, low, mid, high);

        merge(nums, low, mid, high);

        return cnt;
    }

    int reversePairs(vector<int>& nums) {

        return mergeSort(nums, 0, nums.size() - 1);
    }
};
```

# Why Merge Sort?

Brute Force: For every element, check all elements after it.

Time = O(N²)

Too slow.

Merge Sort:

1. Divide array
2. Count reverse pairs between halves
3. Merge halves

Counting takes O(N)

Merge takes O(N)

Depth = log N

Total: O(N log N)