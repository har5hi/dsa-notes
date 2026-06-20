# Majority Element (LeetCode 169)

## Problem Statement

Given an array `nums` of size `n`, return the **majority element**.

The majority element is the element that appears **more than ⌊n/2⌋ times**.

You may assume that the majority element always exists in the array.

### Example

```cpp
Input: nums = [2,2,1,1,1,2,2]
Output: 2
```

---

# Approach 1: Brute Force

For every element, count its frequency by traversing the entire array.

If any element appears more than `n/2` times, return it.

## Algorithm

1. Traverse the array.
2. For each element, count its occurrences.
3. If frequency > `n/2`, return that element.

## Code

```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int n = nums.size();

        for(int i = 0; i < n; i++) {
            int count = 0;

            for(int j = 0; j < n; j++) {
                if(nums[i] == nums[j]) {
                    count++;
                }
            }

            if(count > n / 2) {
                return nums[i];
            }
        }

        return -1;
    }
};
```

### Complexity Analysis

- Time Complexity: **O(n²)**
- Space Complexity: **O(1)**

---

# Approach 2: Hash Map

Store the frequency of each element using a hash map.

## Algorithm

1. Traverse the array and store frequencies.
2. Traverse the map.
3. Return the element whose frequency is greater than `n/2`.

## Code

```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        unordered_map<int, int> mp;

        for(int num : nums) {
            mp[num]++;
        }

        for(auto it : mp) {
            if(it.second > nums.size() / 2) {
                return it.first;
            }
        }

        return -1;
    }
};
```

### Complexity Analysis

- Time Complexity: **O(n)**
- Space Complexity: **O(n)**

---

# Approach 3: Moore's Voting Algorithm (Optimal)

## Intuition

The majority element appears more than `n/2` times.

If we pair one occurrence of the majority element with one occurrence of any other element, the majority element will still remain.

Moore's Voting Algorithm uses this idea of **cancellation**.

---

## Algorithm

1. Initialize:
   - `count = 0`
   - `candidate = 0`

2. Traverse the array:
   - If `count == 0`, choose current element as candidate.
   - If current element equals candidate, increment count.
   - Otherwise, decrement count.

3. The final candidate will be the majority element.

---

## Optimal Code

```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int count = 0;
        int candidate = 0;

        for(int num : nums) {
            if(count == 0) {
                candidate = num;
            }

            if(num == candidate) {
                count++;
            } else {
                count--;
            }
        }

        return candidate;
    }
};
```

### Complexity Analysis

- Time Complexity: **O(n)**
- Space Complexity: **O(1)**

---

# LeetCode 229: Majority Element II

---

# Problem Statement

Given an integer array `nums`, return all elements that appear more than `⌊n/3⌋` times.

---

# Key Observation

For Majority Element I (LC 169):

```cpp
frequency > n/2
```

Only **one** majority element can exist.

For LC 229:

```cpp
frequency > n/3
```

At most **two** majority elements can exist.

Why? Suppose there were 3 elements each occurring more than n/3 times.

Then:

```cpp
n/3 + n/3 + n/3 > n
```
Impossible.

Therefore:

```cpp
Maximum possible majority elements = 2
```

This observation leads to Boyer-Moore Voting Algorithm.

---

# Brute Force Approach

## Idea

For every element:

Count its frequency in the entire array.

If frequency > n/3:

Add to answer.

Avoid duplicates.

---

# Algorithm

```cpp
For each element:
    Count frequency

If frequency > n/3:
    Insert into answer if not already present
```

---

# Code

```cpp
class Solution {
public:
    vector<int> majorityElement(vector<int>& nums) {

        vector<int> ans;
        int n = nums.size();

        for(int i=0;i<n;i++){

            if(find(ans.begin(), ans.end(), nums[i]) != ans.end())
                continue;

            int cnt = 0;

            for(int j=0;j<n;j++){
                if(nums[j] == nums[i])
                    cnt++;
            }

            if(cnt > n/3)
                ans.push_back(nums[i]);
        }

        return ans;
    }
};
```

---

# Complexity Analysis

### Time Complexity

```cpp
O(N²)
```

### Space Complexity

```cpp
O(1)
```

Ignoring output array.

---

# Better Approach (HashMap)

## Idea

Store frequency of each element.

After counting frequencies:

Return elements whose frequency > n/3.

---

# Algorithm

```cpp
unordered_map<int,int> mp

Count frequency

Traverse map:
    if(freq > n/3)
        push into answer
```

---

# Code

```cpp
class Solution {
public:
    vector<int> majorityElement(vector<int>& nums) {

        unordered_map<int,int> mp;

        for(int num : nums)
            mp[num]++;

        vector<int> ans;

        for(auto &it : mp){
            if(it.second > nums.size()/3)
                ans.push_back(it.first);
        }

        return ans;
    }
};
```

---

# Complexity Analysis

### Time Complexity

```cpp
O(N)
```

### Space Complexity

```cpp
O(N)
```

---

# Optimal Approach (Boyer-Moore Voting Algorithm)

---

# Intuition

Since there can be at most 2 majority elements:

Maintain:

```cpp
candidate1
candidate2

count1
count2
```

Whenever we find a new number:

### Case 1

Matches candidate1

```cpp
count1++
```

### Case 2

Matches candidate2

```cpp
count2++
```

### Case 3

count1 == 0

```cpp
candidate1 = current
count1 = 1
```

### Case 4

count2 == 0

```cpp
candidate2 = current
count2 = 1
```

### Case 5

Different from both candidates

```cpp
count1--
count2--
```

This cancels out non-majority elements.

---

# Why Verification Pass Needed?

After first pass:

```cpp
candidate1
candidate2
```

are only potential answers.

Need to count their actual frequencies.

Then check:

```cpp
frequency > n/3
```

---

# Dry Run

Input:

```cpp
[1,2,3,1,1,2,1]
```

n = 7

Need:

```cpp
frequency > 7/3 = 2
```

---

### Start

```cpp
el1 = ?
el2 = ?

cnt1 = 0
cnt2 = 0
```

---

### num = 1

```cpp
cnt1 == 0

el1 = 1
cnt1 = 1
```

---

### num = 2

```cpp
cnt2 == 0

el2 = 2
cnt2 = 1
```

---

### num = 3

Different from both

```cpp
cnt1--
cnt2--
```

Result:

```cpp
cnt1 = 0
cnt2 = 0
```

---

### num = 1

```cpp
el1 = 1
cnt1 = 1
```

---

### num = 1

```cpp
cnt1 = 2
```

---

### num = 2

```cpp
el2 = 2
cnt2 = 1
```

---

### num = 1

```cpp
cnt1 = 3
```

Candidates:

```cpp
el1 = 1
el2 = 2
```

Verification:

```cpp
freq(1) = 4
freq(2) = 2
```

Check:

```cpp
4 > 2 ✓
2 > 2 ✗
```

Answer:

```cpp
[1]
```

---

# Optimal Code

```cpp
class Solution {
public:
    vector<int> majorityElement(vector<int>& nums) {

        int cnt1 = 0, cnt2 = 0;
        int el1 = INT_MIN, el2 = INT_MIN;

        for(int num : nums){

            if(cnt1 == 0 && num != el2){
                cnt1 = 1;
                el1 = num;
            }
            else if(cnt2 == 0 && num != el1){
                cnt2 = 1;
                el2 = num;
            }
            else if(num == el1){
                cnt1++;
            }
            else if(num == el2){
                cnt2++;
            }
            else{
                cnt1--;
                cnt2--;
            }
        }

        cnt1 = cnt2 = 0;

        for(int num : nums){
            if(num == el1) cnt1++;
            else if(num == el2) cnt2++;
        }

        vector<int> ans;
        int mini = nums.size() / 3;

        if(cnt1 > mini) ans.push_back(el1);
        if(cnt2 > mini && el2 != el1)
            ans.push_back(el2);

        return ans;
    }
};
```

---

# Complexity Analysis

### Time Complexity

Two passes through array:

```cpp
O(N)
```

---

### Space Complexity

Only 4 variables:

```cpp
O(1)
```

---

# Interview Follow-Up

### Why only 2 candidates?

Because:

```cpp
More than n/3 times
```

means at most:

```cpp
2 majority elements
```

can exist.

---

### Boyer-Moore Generalization

For:

```cpp
frequency > n/k
```

Keep:

```cpp
k - 1 candidates
```

and

```cpp
k - 1 counts
```

---

# Summary

| Approach           | Time  | Space |
| ------------------ | ----- | ----- |
| Brute Force        | O(N²) | O(1)  |
| HashMap            | O(N)  | O(N)  |
| Boyer-Moore Voting | O(N)  | O(1)  |

---