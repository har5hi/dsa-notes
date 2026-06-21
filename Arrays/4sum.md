# LeetCode 18 - 4Sum

## Problem

Given an array `nums` of `n` integers and an integer `target`, return all unique quadruplets:

```cpp
nums[a] + nums[b] + nums[c] + nums[d] == target
```

where:

```cpp
a, b, c, d are distinct indices
```

---

# Approach 1: Brute Force

## Idea

Generate all possible quadruplets.

Check whether:

```cpp
nums[i] + nums[j] + nums[k] + nums[l] == target
```

Use a set to avoid duplicates.

---

## Algorithm

- Try every combination of 4 indices.
- If sum equals target:
  - Sort quadruplet.
  - Insert into set.
- Convert set into vector.

---

## Complexity

Time:

```cpp
O(N⁴ * log M)
```

Space:

```cpp
O(M)
```

where `M` is number of unique quadruplets.

---

# Approach 2: Better (Hashing)

## Idea

Fix two elements.

Use HashSet to find the remaining two elements.

For each pair:

```cpp
target2 = target - nums[i] - nums[j]
```

Use hashing to find a pair whose sum equals `target2`.

Store answers in a set to remove duplicates.

---

## Complexity

Time:

```cpp
O(N³ log M)
```

Space:

```cpp
O(N + M)
```

---

# Approach 3: Optimal (Sorting + Two Pointers)

## Idea

Sort the array.

Fix two elements:

```cpp
nums[i]
nums[j]
```

Then find remaining two numbers using Two Pointers.

---

## Why Sorting?

After sorting:

### Sum too small

```cpp
left++
```

Need a larger value.

### Sum too large

```cpp
right--
```

Need a smaller value.

---

# Algorithm

1. Sort array.
2. Fix first element `i`.
3. Skip duplicate `i`.
4. Fix second element `j`.
5. Skip duplicate `j`.
6. Use:

```cpp
left = j + 1
right = n - 1
```

7. Compute:

```cpp
sum =
nums[i] +
nums[j] +
nums[left] +
nums[right]
```

### Case 1

```cpp
sum == target
```

Store answer.

Move both pointers.

Skip duplicates.

### Case 2

```cpp
sum < target
```

Move:

```cpp
left++
```

### Case 3

```cpp
sum > target
```

Move:

```cpp
right--
```

---

# Duplicate Handling

## Duplicate i

```cpp
if(i > 0 && nums[i] == nums[i - 1])
    continue;
```

---

## Duplicate j

```cpp
if(j > i + 1 && nums[j] == nums[j - 1])
    continue;
```

---

## Duplicate left

```cpp
while(left < right &&
      nums[left] == nums[left - 1])
{
    left++;
}
```

---

## Duplicate right

```cpp
while(left < right &&
      nums[right] == nums[right + 1])
{
    right--;
}
```

---

# Overflow Handling

Use:

```cpp
long long sum
```

instead of:

```cpp
int sum
```

because:

```cpp
1000000000 +
1000000000 +
1000000000 +
1000000000
```

causes integer overflow.

---

# Optimal Code

```cpp
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {

        vector<vector<int>> ans;
        int n = nums.size();

        sort(nums.begin(), nums.end());

        for(int i = 0; i < n; i++) {

            if(i > 0 && nums[i] == nums[i - 1])
                continue;

            for(int j = i + 1; j < n; j++) {

                if(j > i + 1 && nums[j] == nums[j - 1])
                    continue;

                int left = j + 1;
                int right = n - 1;

                while(left < right) {

                    long long sum =
                        (long long)nums[i] +
                        nums[j] +
                        nums[left] +
                        nums[right];

                    if(sum == target) {

                        ans.push_back({
                            nums[i],
                            nums[j],
                            nums[left],
                            nums[right]
                        });

                        left++;
                        right--;

                        while(left < right &&
                              nums[left] == nums[left - 1])
                            left++;

                        while(left < right &&
                              nums[right] == nums[right + 1])
                            right--;
                    }
                    else if(sum < target) {
                        left++;
                    }
                    else {
                        right--;
                    }
                }
            }
        }

        return ans;
    }
};
```

---

# Complexity

### Time

```cpp
O(N³)
```

### Space

```cpp
O(1)
```

(excluding output)

---

# Pattern to Remember

```text
2Sum:
Fix 0 elements
Use 2 pointers

O(N)

3Sum:
Fix 1 element
Use 2 pointers

O(N²)

4Sum:
Fix 2 elements
Use 2 pointers

O(N³)

kSum:
Fix (k-2) elements
Solve remaining using 2Sum
``