# 540. Single Element in a Sorted Array

## Problem Statement

Given a sorted array where:

- Every element appears exactly twice.
- Only one element appears once.

Return the single element.

---

# Brute Force Approach

### Idea

Count the frequency of every element using a hashmap.
The element with frequency `1` is the answer.

### Code

```cpp
class Solution {
public:
    int singleNonDuplicate(vector<int>& nums) {

        unordered_map<int,int> mp;

        for(int num : nums)
            mp[num]++;

        for(auto it : mp) {
            if(it.second == 1)
                return it.first;
        }

        return -1;
    }
};
```

### Complexity Analysis

| Complexity | Value |
|------------|--------|
| Time | O(n) |
| Space | O(n) |

---

# Better Approach (XOR)

## Observation

Properties of XOR:

```cpp
a ^ a = 0
```

```cpp
a ^ 0 = a
```

Since every element appears twice:

```text
1 ^ 1 = 0
3 ^ 3 = 0
4 ^ 4 = 0
```

Only the single element remains.

---

## Algorithm

1. Initialize `xorAns = 0`.
2. XOR every element.
3. Return result.

---

## Code

```cpp
class Solution {
public:
    int singleNonDuplicate(vector<int>& nums) {

        int xorAns = 0;

        for(int num : nums)
            xorAns ^= num;

        return xorAns;
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

Since the array is sorted:
Before the single element:

```text
1 1 2 2 3 3 4 5 5
      ^
```

Pairs start at **even indices**.

```text
Index: 0 1 2 3 4 5
Value: 1 1 2 2 3 3
```

---

After the single element appears:

```text
1 1 2 3 3 4 4
      ^
```

The pairing pattern shifts.

```cpp
odd index -> first occurrence
even index -> second occurrence
```

This makes Binary Search possible.

---

# Important Trick

For any index:

### Even Index

If:

```cpp
mid % 2 == 0
```

Expected pair:

```cpp
nums[mid] == nums[mid + 1]
```

If true:

```cpp
answer lies on right
```

Otherwise:

```cpp
answer lies on left including mid
```

---

### Odd Index

Expected pair:

```cpp
nums[mid] == nums[mid - 1]
```

If true:

```cpp
answer lies on right
```

Otherwise:

```cpp
answer lies on left
```

---

# Algorithm

1. Handle edge cases.
2. Apply Binary Search.
3. Check pairing pattern.
4. Eliminate half of the array.
5. Return the single element.

---

# Optimal Code

```cpp
class Solution {
public:
    int singleNonDuplicate(vector<int>& nums) {

        int n = nums.size();

        if(n == 1)
            return nums[0];

        if(nums[0] != nums[1])
            return nums[0];

        if(nums[n - 1] != nums[n - 2])
            return nums[n - 1];

        int low = 1;
        int high = n - 2;

        while(low <= high) {

            int mid = low + (high - low) / 2;

            if(nums[mid] != nums[mid - 1] &&
               nums[mid] != nums[mid + 1])
                return nums[mid];

            if((mid % 2 == 1 && nums[mid] == nums[mid - 1]) ||
               (mid % 2 == 0 && nums[mid] == nums[mid + 1])) {

                low = mid + 1;
            }
            else {
                high = mid - 1;
            }
        }

        return -1;
    }
};
```

---

# Why It Works

Before the single element:

```text
even -> first occurrence
odd  -> second occurrence
```

After the single element:

```text
odd  -> first occurrence
even -> second occurrence
```

The single element causes the pairing pattern to shift.

Binary Search detects where the shift occurs and narrows the search space.

---

# Complexity Analysis

| Complexity | Value |
|------------|--------|
| Time | O(log n) |
| Space | O(1) |

---