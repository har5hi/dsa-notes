# LeetCode 15 - 3Sum

## Problem

Given an integer array `nums`, return all unique triplets:

```cpp
nums[i] + nums[j] + nums[k] == 0
```

where:

```cpp
i != j , j != k & i != k
```

---

# Approach 1: Brute Force

## Idea

Generate all possible triplets and check if their sum is 0.

Use a set to avoid duplicates.

## Algorithm

- Try every combination of 3 indices.
- If sum is 0:
  - Sort the triplet.
  - Insert into set.
- Convert set into vector.

## Complexity

Time: `O(N³ * log M)`

Space: `O(M)`

where `M` = number of unique triplets.

---

# Approach 2: Better (Hashing)

## Idea

Fix one element and use HashSet to find the remaining pair.

For every `i`:

- Create a HashSet.
- Find two numbers whose sum equals `-nums[i]`.

## Algorithm

For each index:

```cpp
target = -nums[i]
```

For every `j > i`:

```cpp
third = target - nums[j]
```

If third exists in HashSet:

Triplet found.

Store sorted triplet in set.

Else insert nums[j].

## Complexity

Time: `O(N² log M)`

Space: `O(N + M)`

---

# Approach 3: Optimal (Sorting + Two Pointers)

## Idea

Sort the array.

Fix one element and find remaining two elements using Two Pointers.

### Why Sorting?

After sorting:

- If sum < 0 → move left pointer.
- If sum > 0 → move right pointer.

This allows finding pairs in linear time.

---

## Algorithm

1. Sort array.
2. Fix element `nums[i]`.
3. Skip duplicate `i`.
4. Use:

```cpp
left = i + 1
right = n - 1
```

5. While left < right:

```cpp
sum = nums[i] + nums[left] + nums[right]
```

### Case 1

```cpp
sum == 0
```

Store triplet.

Skip duplicates.

### Case 2

```cpp
sum < 0
```

Move:

```cpp
left++
```

### Case 3

```cpp
sum > 0
```

Move:

```cpp
right--
```

---

## Duplicate Handling

### Duplicate Fixed Element

```cpp
if(i > 0 && nums[i] == nums[i-1])
    continue;
```

### Duplicate Left

```cpp
while(left < right &&
      nums[left] == nums[left-1])
{
    left++;
}
```

### Duplicate Right

```cpp
while(left < right &&
      nums[right] == nums[right+1])
{
    right--;
}
```

---

## Code

```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {

        vector<vector<int>> result;

        sort(nums.begin(), nums.end());

        for(int i = 0; i < nums.size(); i++) {

            if(i > 0 && nums[i] == nums[i - 1])
                continue;

            int left = i + 1;
            int right = nums.size() - 1;

            while(left < right) {

                int sum = nums[i] + nums[left] + nums[right];

                if(sum == 0) {

                    result.push_back(
                        {nums[i], nums[left], nums[right]}
                    );

                    left++;
                    right--;

                    while(left < right &&
                          nums[left] == nums[left - 1])
                        left++;

                    while(left < right &&
                          nums[right] == nums[right + 1])
                        right--;
                }
                else if(sum < 0) {
                    left++;
                }
                else {
                    right--;
                }
            }
        }

        return result;
    }
};
```

---

## Complexity

### Time

Sorting:

```cpp
O(N log N)
```

Two Pointer Traversal:

```cpp
O(N²)
```

Overall:

```cpp
O(N²)
```

### Space

```cpp
O(1)
```

(excluding output array)

---
