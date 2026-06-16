# Sort Colors (LeetCode 75)

## Problem Statement

Given an array `nums` containing only `0`, `1`, and `2`, sort the array in-place so that objects of the same color are adjacent, with the colors in the order:

`0 → 1 → 2`

---

# Approach 1: Counting Sort

## Intuition

Since the array contains only three distinct values (`0`, `1`, and `2`), we can:

1. Count the occurrences of each number.
2. Overwrite the array with:
   - all `0`s first
   - then all `1`s
   - then all `2`s

---

## Algorithm

1. Traverse the array and count:
   - `count0`
   - `count1`
   - `count2`
2. Fill the first `count0` positions with `0`.
3. Fill the next `count1` positions with `1`.
4. Fill the remaining positions with `2`.

---

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;

void sortNum(int *nums, int n){
    int count0 = 0;
    int count1 = 0;
    int count2 = 0;

    for(int i = 0; i < n; i++){
        if(nums[i]==0) {count0++;}
        else if(nums[i]==1) {count1++;}
        else {count2++;}
    }
    
    for(int i = 0; i<count0; i++){
        nums[i] = 0;
    }
    for(int i = count0; i<count0+count1; i++){
        nums[i] = 1;
    }
    for(int i = count0+count1; i<n; i++){
        nums[i] = 2;
    }
}

int main() {
   int nums[] = {2,0,2,1,1,0};
   int n = sizeof(nums)/sizeof(nums[0]);
   
   sortNum(nums, n);

   cout << "Final Sorted Array: ";
   for(int i = 0; i < n; i++) {
     cout << nums[i] << " ";
   }
   
   return 0;
}
```

---

## Complexity Analysis

- Time Complexity: **O(n)**
- Space Complexity: **O(1)**

---

# Approach 2: Dutch National Flag Algorithm (Optimal)

## Intuition

Instead of counting first and rewriting later, we can sort the array in a single traversal using three pointers.

Maintain:

- `low` → boundary for `0`s
- `mid` → current element being processed
- `high` → boundary for `2`s

### Regions

```cpp
0 to low-1      -> 0s
low to mid-1    -> 1s
mid to high     -> unsorted
high+1 to n-1   -> 2s
```

---

## Algorithm

While `mid <= high`:

### Case 1: nums[mid] == 0

Swap with `low`.

```cpp
swap(nums[mid], nums[low]);
low++;
mid++;
```

### Case 2: nums[mid] == 1

Already in correct position.

```cpp
mid++;
```

### Case 3: nums[mid] == 2

Swap with `high`.

```cpp
swap(nums[mid], nums[high]);
high--;
```

Do **not** increment `mid` here because the swapped element from the right side still needs processing.

---

## Code

```cpp
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int low = 0;
        int mid = 0;
        int high = nums.size() - 1;

        while(mid <= high) {

            if(nums[mid] == 0) {
                swap(nums[mid], nums[low]);
                low++;
                mid++;
            }

            else if(nums[mid] == 1) {
                mid++;
            }

            else {
                swap(nums[mid], nums[high]);
                high--;
            }
        }
    }
};
```

---

## Complexity Analysis

### Time Complexity

```cpp
O(n)
```

Each element is processed at most once.

### Space Complexity

```cpp
O(1)
```

Only three pointers are used.

---

# Comparison

| Approach | Time | Space | Passes |
|-----------|------|--------|---------|
| Counting Sort | O(n) | O(1) | 2 |
| Dutch National Flag | O(n) | O(1) | 1 |

---

# Key Interview Takeaway

The Dutch National Flag Algorithm is the expected optimal solution because:

- Single traversal
- Constant extra space
- In-place sorting
- Demonstrates strong understanding of pointer manipulation