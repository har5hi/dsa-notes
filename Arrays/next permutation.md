# LeetCode 31 - Next Permutation

## Problem Statement

Given an array of integers `nums`, rearrange the numbers into the **next lexicographically greater permutation**.

If such an arrangement is not possible (i.e., the array is in descending order), rearrange it as the **lowest possible order (ascending order)**.

The modification must be done **in-place** and use only **constant extra memory**.

---

## Key Observation

To get the next permutation:

1. Find the first index from the right where:

   ```cpp
   nums[i] < nums[i+1]
   ```

   This is called the **breakpoint**.

2. If no breakpoint exists:

   * The array is in descending order.
   * Reverse the entire array.

3. Otherwise:

   * Find the first element from the right that is greater than `nums[ind]`.
   * Swap them.

4. Reverse the suffix after the breakpoint to get the smallest possible arrangement.

---

## Example

### Input

```cpp
[1,2,3]
```

### Step 1: Find Breakpoint

```cpp
2 < 3
```

Breakpoint:

```cpp
ind = 1
```

### Step 2: Find Next Greater Element

```cpp
3 > 2
```

Swap:

```cpp
[1,3,2]
```

### Step 3: Reverse Suffix

Suffix:

```cpp
[2]
```

Result:

```cpp
[1,3,2]
```

---

## Dry Run

### Input

```cpp
[1,3,2]
```

Breakpoint:

```cpp
1 < 3
ind = 0
```

Find next greater element from right:

```cpp
2 > 1
```

Swap:

```cpp
[2,3,1]
```

Reverse suffix:

```cpp
[3,1] → [1,3]
```

Final Answer:

```cpp
[2,1,3]
```

---

## Why Reverse the Suffix?

After finding the breakpoint:

* The part to the right of the breakpoint is always in **descending order**.
* After swapping, we need the **smallest possible arrangement** for that suffix.
* Reversing a descending sequence converts it into ascending order.

This guarantees the immediate next permutation.

---

## Optimal Code

```cpp
class Solution {
public:
    void nextPermutation(vector<int>& nums) {
        int n = nums.size();
        int ind = -1;

        // Find breakpoint
        for(int i = n - 2; i >= 0; i--) {
            if(nums[i] < nums[i + 1]) {
                ind = i;
                break;
            }
        }

        // If no breakpoint exists
        if(ind == -1) {
            reverse(nums.begin(), nums.end());
            return;
        }

        // Find next greater element
        for(int i = n - 1; i > ind; i--) {
            if(nums[i] > nums[ind]) {
                swap(nums[i], nums[ind]);
                break;
            }
        }

        // Reverse suffix
        reverse(nums.begin() + ind + 1, nums.end());
    }
};
```

---

## Complexity Analysis

### Time Complexity

* Find breakpoint → `O(n)`
* Find next greater element → `O(n)`
* Reverse suffix → `O(n)`

Overall:

```cpp
O(n)
```

### Space Complexity

```cpp
O(1)
```

---

## Interview Tip

Remember the 3-step pattern:

```text
Find Breakpoint
       ↓
Find Next Greater Element
       ↓
Reverse Suffix
```

This pattern is frequently asked in interviews and competitive programming.