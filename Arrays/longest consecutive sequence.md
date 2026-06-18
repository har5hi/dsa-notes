# LeetCode 128 - Longest Consecutive Sequence

## Problem Statement

Given an unsorted array of integers `nums`, return the length of the longest consecutive elements sequence.

You must write an algorithm that runs in **O(n)** time.

### Example

```cpp
Input: nums = [100,4,200,1,3,2]

Output: 4
```

Explanation:

```cpp
[1,2,3,4]
```

is the longest consecutive sequence, so the answer is `4`.

---

# Brute Force Approach

For every element:

1. Check if `num + 1` exists.
2. Continue counting until the sequence breaks.
3. Store the maximum length.

### Complexity

```cpp
Time: O(n²)
Space: O(1)
```

Too slow for large inputs.

---

# Optimal Approach (HashSet)

## Key Idea

Store all numbers in an `unordered_set`.

A number can be the **start of a sequence** only if:

```cpp
num - 1
```

does not exist in the set.

Example:

```cpp
[1,2,3,4]
```

* 1 → start of sequence
* 2 → not a start (1 exists)
* 3 → not a start (2 exists)
* 4 → not a start (3 exists)

This ensures every sequence is counted only once.

---

# Algorithm

### Step 1

Insert all elements into a HashSet.

```cpp
unordered_set<int> numSet(nums.begin(), nums.end());
```

---

### Step 2

Traverse every number in the set.

```cpp
for(int num : numSet)
```

---

### Step 3

Check whether it is the beginning of a sequence.

```cpp
if(numSet.find(num - 1) == numSet.end())
```

If previous number does not exist, start counting.

---

### Step 4

Keep extending the sequence.

```cpp
while(numSet.find(num + length) != numSet.end())
{
    length++;
}
```

---

### Step 5

Update the answer.

```cpp
longest = max(longest, length);
```

---

# Dry Run

### Input

```cpp
nums = [100,4,200,1,3,2]
```

Set:

```cpp
{100,4,200,1,3,2}
```

---

### num = 100

Check:

```cpp
99 not present
```

Start sequence.

```cpp
100
```

Length:

```cpp
1
```

Longest:

```cpp
1
```

---

### num = 4

Check:

```cpp
3 exists
```

Not a starting point.

Skip.

---

### num = 200

Check:

```cpp
199 not present
```

Start sequence.

Length:

```cpp
1
```

Longest:

```cpp
1
```

---

### num = 1

Check:

```cpp
0 not present
```

Start sequence.

```cpp
1 → 2 → 3 → 4
```

Length:

```cpp
4
```

Longest:

```cpp
4
```

---

### Final Answer

```cpp
4
```

---

# Why Does This Work?

Without the condition:

```cpp
numSet.find(num - 1) == numSet.end()
```

we would start counting from every number:

```cpp
1 → 2 → 3 → 4
2 → 3 → 4
3 → 4
4
```

leading to unnecessary work.

By only starting from the first element of a sequence, every number is processed at most once.

---

# Optimal Code

```cpp
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        unordered_set<int> numSet(nums.begin(), nums.end());

        int longest = 0;

        for(int num : numSet) {

            // Start of a sequence
            if(numSet.find(num - 1) == numSet.end()) {

                int length = 1;

                while(numSet.find(num + length) != numSet.end()) {
                    length++;
                }

                longest = max(longest, length);
            }
        }

        return longest;
    }
};
```

---

# Complexity Analysis

### Time Complexity

Building HashSet:

```cpp
O(n)
```

Traversing elements:

```cpp
O(n)
```

Overall:

```cpp
O(n)
```

Each number is visited at most once while expanding sequences.

---

### Space Complexity

HashSet stores all elements:

```cpp
O(n)
```

---

# Interview Tip

The most important observation is:

```text
Only start counting when the previous number
(num - 1) does not exist.
```

This avoids recounting the same sequence multiple times and reduces the complexity from **O(n²)** to **O(n)**.