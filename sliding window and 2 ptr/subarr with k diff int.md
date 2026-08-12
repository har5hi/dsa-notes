# LeetCode 992 - Subarrays with K Different Integers

**Difficulty:** Hard

**Topic:** Array, HashMap, Sliding Window, Two Pointers

---

# Problem Statement

Given an integer array `nums` and an integer `k`,

return the **number of good subarrays**.

A **good subarray** is a contiguous subarray that contains **exactly `k` distinct integers**.

---

# Example 1

```text
Input

nums = [1,2,1,2,3]

k = 2
```

Output

```text
7
```

Explanation

Valid subarrays

```text
[1,2]

[2,1]

[1,2]

[2,3]

[1,2,1]

[2,1,2]

[1,2,1,2]
```

Total = 7

---

# Example 2

```text
Input

nums = [1,2,1,3,4]

k = 3
```

Output

```text
3
```

Valid subarrays

```text
[1,2,1,3]

[2,1,3]

[1,3,4]
```

---

# Understanding the Problem

We need to count every subarray that contains

```text
Exactly K Different Numbers
```

Example

```
1 2 1 2 3
```

Valid

```
1 2

2 1

1 2 1

2 1 2

1 2 1 2
```

Invalid

```
1

2

1 2 1 2 3
```

because

```
Distinct = 1

or

Distinct = 3
```

---

# Key Observation

Counting

```text
Exactly K
```

directly is difficult.

Instead,

count

```text
At Most K Distinct
```

and

```text
At Most (K-1) Distinct
```

Then subtract.

---

# The Formula

```text
Exactly(K)

=

AtMost(K)

-

AtMost(K-1)
```

This is the most important observation.

---

# Why does this formula work?

Suppose

```
AtMost(2)
```

contains

```
Distinct = 1

and

Distinct = 2
```

Suppose

```
AtMost(1)
```

contains only

```
Distinct = 1
```

Subtracting

```
AtMost(2)

-

AtMost(1)
```

removes all subarrays having

```
1 distinct
```

leaving only

```
2 distinct
```

Exactly what we need.

---

# Approaches

---

# 1. Brute Force

## Intuition

Generate every possible subarray.

For every subarray,

count distinct elements using a HashSet.

If distinct count becomes exactly `k`,

increase answer.

If it becomes greater than `k`,

stop checking that starting index.

---

# Algorithm

1. Start from every index.
2. Keep inserting elements into a HashSet.
3. If distinct count equals `k`,
   increment answer.
4. If distinct count becomes greater than `k`,
   break.

---

# Brute Force Code

```cpp
class Solution {
public:

    int subarraysWithKDistinct(vector<int>& nums, int k) {

        int ans = 0;
        int n = nums.size();

        for(int i = 0; i < n; i++) {

            unordered_set<int> st;

            for(int j = i; j < n; j++) {

                st.insert(nums[j]);

                if(st.size() == k)
                    ans++;

                else if(st.size() > k)
                    break;
            }
        }

        return ans;
    }
};
```

---

# Complexity

Time

```text
O(n²)
```

Space

```text
O(k)
```

---

# Why Brute Force is Slow?

For every starting index,

we keep creating new subarrays.

Example

```
1

↓

1 2

↓

1 2 1

↓

1 2 1 2
```

Then start again

```
2

↓

2 1

↓

2 1 2
```

Lots of repeated work.

---

# 2. Optimal Approach (Sliding Window)

⭐ Most Interview Preferred

---

# Intuition

Instead of counting

```text
Exactly K
```

directly,

count

```text
At Most K
```

using Sliding Window.

Then use

```cpp
Exactly(K)

=

AtMost(K)

-

AtMost(K-1)
```

---

# How does AtMost(K) work?

Maintain a window having

```
Distinct ≤ K
```

Whenever distinct becomes greater than `K`,

shrink the window.

Every valid window contributes

```cpp
right-left+1
```

subarrays.

---

# Why

```cpp
ans += right-left+1;
```

Suppose

Current window

```
1 2 1

^

left

      ^

     right
```

Valid subarrays ending at `right`

```
1

2 1

1 2 1
```

There are

```
right-left+1
```

such subarrays.

---

# Algorithm

### atMost(K)

1. Expand the window.
2. Store frequencies in a HashMap.
3. If distinct elements become greater than `K`,
   shrink the window.
4. Add

```cpp
right-left+1
```

to the answer.
5. Return answer.

Finally,

```cpp
Exactly(K)

=

AtMost(K)

-

AtMost(K-1)
```

---

# Optimal Code (Fully Commented)

```cpp
class Solution {
public:

    // Counts subarrays having at most k distinct integers
    int atMost(vector<int>& nums, int k) {

        if(k < 0)
            return 0;

        unordered_map<int,int> freq;

        int left = 0;
        int ans = 0;

        for(int right = 0; right < nums.size(); right++) {

            // Include current element
            freq[nums[right]]++;

            // Shrink window until distinct <= k
            while(freq.size() > k) {

                freq[nums[left]]--;

                // Remove element completely if frequency becomes zero
                if(freq[nums[left]] == 0)
                    freq.erase(nums[left]);

                left++;
            }

            // Count all valid subarrays ending at right
            ans += (right-left+1);
        }

        return ans;
    }

    int subarraysWithKDistinct(vector<int>& nums, int k) {

        return atMost(nums,k) - atMost(nums,k-1);
    }
};
```

---

# Complexity

### Time

```text
O(n)
```

Each element enters and leaves the window only once.

---

### Space

```text
O(k)
```

HashMap stores at most `k+1` distinct elements.

---

# Why Sliding Window Works

The window always satisfies

```text
Distinct ≤ K
```

Whenever it becomes invalid,

we move `left` until it becomes valid again.

For every valid window,

there are

```cpp
right-left+1
```

subarrays ending at `right`.

Thus,

the answer is computed in linear time.

---

# Understanding the Optimal Solution (Sliding Window)

Before understanding the code, remember one important observation.

We are **NOT counting subarrays with exactly `k` distinct elements directly.**

Instead, we count:

```text
Subarrays with At Most K Distinct Elements
```

and

```text
Subarrays with At Most (K-1) Distinct Elements
```

Then,

```text
Exactly K

=

At Most K

-

At Most (K-1)
```

This is the entire idea behind the optimal solution.

---

# Complete Code

```cpp
class Solution {
public:

    // Returns number of subarrays having at most k distinct integers
    int atMost(vector<int>& nums, int k) {

        // Impossible case
        if(k < 0)
            return 0;

        unordered_map<int,int> freq;

        int left = 0;
        int ans = 0;

        for(int right = 0; right < nums.size(); right++) {

            // Include current element in the window
            freq[nums[right]]++;

            // Shrink window until distinct elements <= k
            while(freq.size() > k) {

                freq[nums[left]]--;

                // Remove the element completely if its frequency becomes zero
                if(freq[nums[left]] == 0)
                    freq.erase(nums[left]);

                left++;
            }

            // Count all valid subarrays ending at 'right'
            ans += (right - left + 1);
        }

        return ans;
    }

    int subarraysWithKDistinct(vector<int>& nums, int k) {

        return atMost(nums, k) - atMost(nums, k - 1);
    }
};
```

---

# Understanding Every Line

---

## Line 1

```cpp
if(k < 0)
    return 0;
```

Suppose someone asks

```
At Most -1 Distinct Elements
```

This is impossible.

So,

```
Return 0
```

---

## Line 2

```cpp
unordered_map<int,int> freq;
```

Stores the frequency of every element inside the current window.

Example

Window

```
1 2 1
```

HashMap

```
1 → 2

2 → 1
```

This tells us

- which elements are inside the window
- how many times they appear

---

## Line 3

```cpp
int left = 0;
```

Left boundary of the sliding window.

Initially

```
1 2 1 2 3

^

left
```

---

## Line 4

```cpp
int ans = 0;
```

Stores the total number of valid subarrays.

Initially

```
0
```

---

## Line 5

```cpp
for(int right = 0; right < nums.size(); right++)
```

Expand the window one element at a time.

Example

```
1

↓

1 2

↓

1 2 1

↓

1 2 1 2
```

---

## Line 6

```cpp
freq[nums[right]]++;
```

Current element enters the window.

Example

Window

```
1 2
```

Map

```
1 → 1

2 → 1
```

Now

```
right
```

points to another

```
1
```

Map becomes

```
1 → 2

2 → 1
```

Notice,

the number of **distinct elements** remains **2** because `1` was already present.

---

## The Most Important Line

```cpp
while(freq.size() > k)
```

`freq.size()` tells us

```
How many distinct numbers
```

are currently inside the window.

Suppose

Window

```
1 2 1 3
```

Map

```
1 → 2

2 → 1

3 → 1
```

Distinct elements

```
3
```

If

```
k = 2
```

this window is invalid.

So we shrink it.

---

## Line 8

```cpp
freq[nums[left]]--;
```

Remove one occurrence of the leftmost element.

Example

Window

```
1 2 1 3
```

Map

```
1 → 2

2 → 1

3 → 1
```

Remove first

```
1
```

Map

```
1 → 1

2 → 1

3 → 1
```

Still present.

So distinct count is still

```
3
```

---

## Line 9

```cpp
if(freq[nums[left]] == 0)
    freq.erase(nums[left]);
```

Suppose instead

Window

```
2 1 3
```

Map

```
2 → 1

1 → 1

3 → 1
```

Remove

```
2
```

Frequency becomes

```
2 → 0
```

Since it no longer exists inside the window,

remove it completely.

Map becomes

```
1 → 1

3 → 1
```

Now

```
freq.size()

=

2
```

Window becomes valid again.

---

## Line 10

```cpp
left++;
```

Move the left boundary.

Before

```
1 2 1 3

^

left
```

After

```
1 2 1 3

  ^
```

---

# The Heart of Sliding Window

```cpp
ans += (right-left+1);
```

Many students memorize this.

Don't.

Understand it.

---

Suppose current window is

```
1 2 1

^

left

      ^

     right
```

Every subarray ending at `right` is

```
1

2 1

1 2 1
```

Count

```
3
```

Formula

```
right-left+1

=

2-0+1

=

3
```

Exactly correct.

---

Another Example

Window

```
2 3 4 5
```

Valid subarrays ending at

```
5
```

are

```
5

4 5

3 4 5

2 3 4 5
```

Count

```
4
```

Formula

```
right-left+1

=

4
```

---

# Why Does This Always Work?

Suppose

```
left = 2

right = 6
```

Window length

```
6-2+1

=

5
```

Possible starting positions

```
2

3

4

5

6
```

Exactly

```
5
```

Each starting position creates one valid subarray ending at `right`.

Hence

```cpp
ans += right-left+1;
```

---

# Why

Exactly(K)

=

AtMost(K)

-

AtMost(K-1)

?

Suppose

```
nums

=

1 2 1
```

### AtMost(2)

Contains

```
Distinct = 1

Distinct = 2
```

Suppose answer

```
6
```

### AtMost(1)

Contains only

```
Distinct = 1
```

Suppose answer

```
3
```

Subtract

```
6-3

=

3
```

Remaining subarrays have

```
Exactly 2 Distinct
```

---

# Complete Dry Run

Input

```
nums = [1,2,1,2,3]

k = 2
```

Initially

```
left = 0

right = 0

ans = 0
```

---

## right = 0

Window

```
1
```

Distinct

```
1
```

Valid

Add

```
1
```

Answer

```
1
```

---

## right = 1

Window

```
1 2
```

Distinct

```
2
```

Valid

Add

```
2
```

Answer

```
3
```

Subarrays

```
2

1 2
```

---

## right = 2

Window

```
1 2 1
```

Distinct

```
2
```

Add

```
3
```

Answer

```
6
```

Subarrays

```
1

2 1

1 2 1
```

---

## right = 3

Window

```
1 2 1 2
```

Distinct

```
2
```

Add

```
4
```

Answer

```
10
```

---

## right = 4

Window

```
1 2 1 2 3
```

Distinct

```
3
```

Invalid

Shrink

Remove

```
1
```

Still

```
3 distinct
```

Remove

```
2
```

Still

```
3 distinct
```

Remove

```
1
```

Now

Window

```
2 3
```

Distinct

```
2
```

Valid

Add

```
2
```

Final

```
AtMost(2)=12
```

Similarly,

```
AtMost(1)=5
```

Answer

```
12-5

=

7
```

Correct.

---

# Visual Representation

```
1 2 1 2

↓

Valid

↓

Add

right-left+1
```

Whenever a third distinct number enters

```
1 2 1 2 3
```

Shrink

```
↓

2 3
```

Continue.

---

# Interview Tips

### ✅ Observation 1

Whenever the question asks

```
Exactly K
```

think

```text
Exactly

=

AtMost

-

AtMost
```

---

### ✅ Observation 2

Whenever the question involves

```
Distinct Elements
```

use

```cpp
unordered_map
```

to maintain frequencies.

---

### ✅ Observation 3

Remember the three sliding window counting patterns:

**Pattern 1**

```cpp
maxLen = max(maxLen, right-left+1);
```

Used for longest window problems.

---

**Pattern 2**

```cpp
ans += right-left+1;
```

Counts all valid subarrays ending at `right`.

---

**Pattern 3**

```cpp
ans += n-right;
```

Counts all valid substrings starting at `left`.

---