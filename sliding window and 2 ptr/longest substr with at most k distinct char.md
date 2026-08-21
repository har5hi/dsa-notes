# Longest Substring with At Most K Distinct Characters

---

# Problem Statement

Given a string `s` and an integer `k`, return the length of the **longest substring** that contains **at most `k` distinct characters**.

A substring must be **contiguous**.

---

# Example 1

```text
Input:

s = "eceba"
k = 2
```

Output:

```text
3
```

Explanation:

The longest substring with at most 2 distinct characters is:

```text
"ece"
```

Distinct characters:

```text
e
c
```

Length:

```text
3
```

---

# Example 2

```text
Input:

s = "aa"
k = 1
```

Output:

```text
2
```

The entire string contains only one distinct character:

```text
a
```

So the answer is:

```text
2
```

---

# Main Idea

1. Expand the window using `right`.
2. Add the current character to a frequency map.
3. If the number of distinct characters becomes greater than `k`,
   move `left` forward.
4. Continue shrinking until the window becomes valid.
5. Update the maximum length.

The important formula is:

```cpp
right - left + 1
```

which gives the current window length.

---

# Approaches

---

# 1. Brute Force

## Intuition

Generate every possible substring.

For each substring:

- Keep track of its distinct characters.
- If the number of distinct characters is at most `k`, update the answer.

---

# Brute Force Code

```cpp
class Solution {
public:

    int longestSubstring(string s, int k) {

        int n = s.length();
        int maxLen = 0;

        for(int i = 0; i < n; i++) {

            unordered_map<char,int> freq;

            for(int j = i; j < n; j++) {
                freq[s[j]]++;

                if(freq.size() <= k) {
                    maxLen = max(maxLen, j-i+1);
                }
                else {
                    break;
                }
            }
        }
        return maxLen;
    }
};
```

---

# Complexity

Time:

```text
O(n²)
```

Space:

```text
O(k)
```

---

# 2. Optimal Approach - Sliding Window

---

# Intuition

Instead of generating every substring,

maintain only one window.

Example:

```text
s = "eceba"
k = 2
```

Start:

```text
e
```

Expand:

```text
ec
```

Expand:

```text
ece
```

Still valid because there are only:

```text
2 distinct characters
```

Expand:

```text
eceb
```

Now there are:

```text
e
c
b
```

So:

```text
3 distinct > 2
```

Window is invalid.

Shrink from the left:

```text
ce
```

Then continue expanding.

---

# Algorithm

1. Create a frequency map.
2. Set:
   ```cpp
   left = 0
   maxLen = 0
   ```
3. Move `right` from left to right.
4. Add `s[right]` to the frequency map.
5. While distinct characters are greater than `k`:
   - Remove `s[left]`.
   - If its frequency becomes 0, erase it.
   - Move `left`.
6. Update:
   ```cpp
   maxLen = max(maxLen, right-left+1);
   ```
7. Return `maxLen`.

---

# Optimal Code

```cpp
class Solution {
public:

    int longestSubstring(string s, int k) {

        // Stores frequency of each character
        unordered_map<char, int> freq;

        // Left boundary of the window
        int left = 0;

        // Stores the longest valid window
        int maxLen = 0;

        // Expand the window using right
        for(int right = 0; right < s.length(); right++) {

            // Add current character to the window
            freq[s[right]]++;

            // Shrink window if distinct characters exceed k
            while(freq.size() > k) {

                // Remove leftmost character
                freq[s[left]]--;

                // If character is no longer present,
                // remove it from the map
                if(freq[s[left]] == 0) {
                    freq.erase(s[left]);
                }

                // Move left pointer
                left++;
            }

            // Current window is valid
            maxLen = max(maxLen, right-left+1);
        }
        return maxLen;
    }
};
```

---

# Complexity

### Time Complexity

```text
O(n)
```

Both `left` and `right` move only forward.

Each character:

- enters the window once
- leaves the window at most once

Therefore:

```text
O(n)
```

---

### Space Complexity

```text
O(k)
```

The HashMap stores the distinct characters in the current window.

If the character set is fixed, this can effectively be considered:

```text
O(1)
```

---

# Detailed Explanation

## Frequency Map

```cpp
unordered_map<char,int> freq;
```

Suppose current window is:

```text
ece
```

The map stores:

```text
e → 2
c → 1
```

Notice that:

```cpp
freq.size()
```

is:

```text
2
```

because there are two distinct characters.

It is **not** the total number of characters.

---

# `left` Pointer

```cpp
int left = 0;
```

This represents the beginning of our current window.

For:

```text
e c e b a
^
left
```

Initially:

```text
left = 0
```

---

# `right` Pointer

```cpp
for(int right = 0; right < s.length(); right++)
```

`right` expands the window.

For example:

```text
e

↓

ec

↓

ece

↓

eceb
```

---

# Adding a Character

```cpp
freq[s[right]]++;
```

Suppose:

```text
window = "ece"
```

Frequency:

```text
e → 2
c → 1
```

Now `right` reaches `b`.

After:

```cpp
freq['b']++;
```

Map becomes:

```text
e → 2
c → 1
b → 1
```

Distinct characters:

```text
3
```

---

# Checking the Window

```cpp
while(freq.size() > k)
```

Suppose:

```text
k = 2
```

Current distinct characters:

```text
3
```

Therefore:

```text
3 > 2
```

The window is invalid.

We must shrink it.

---

# Removing From the Left

```cpp
freq[s[left]]--;
```

Suppose:

```text
window = "eceb"
```

and:

```text
left
 ↓
e c e b
```

Remove the first `e`.

Frequency becomes:

```text
e → 1
c → 1
b → 1
```

`e` is still present, so we don't erase it.

---

# When Do We Erase?

```cpp
if(freq[s[left]] == 0)
    freq.erase(s[left]);
```

Suppose the window is:

```text
e b
```

and we remove `e`.

Frequency:

```text
e → 0
```

There is no `e` left in the window.

So we erase it:

```cpp
freq.erase('e');
```

Now:

```text
freq.size()
```

decreases.

---

# Updating the Answer

```cpp
maxLen = max(maxLen, right-left+1);
```

This calculates the current window length.

Example:

```text
left = 1
right = 3
```

Length:

```text
3 - 1 + 1

= 3
```

Then compare with the previous maximum.

---

# Complete Dry Run

Input:

```text
s = "eceba"

k = 2
```

Initially:

```text
left = 0
maxLen = 0
```

---

## Step 1: `right = 0`

Character:

```text
e
```

Window:

```text
e
```

Frequency:

```text
e → 1
```

Distinct:

```text
1
```

Valid.

Length:

```text
1
```

Answer:

```text
maxLen = 1
```

---

## Step 2: `right = 1`

Character:

```text
c
```

Window:

```text
ec
```

Frequency:

```text
e → 1
c → 1
```

Distinct:

```text
2
```

Valid.

Length:

```text
2
```

Answer:

```text
maxLen = 2
```

---

## Step 3: `right = 2`

Character:

```text
e
```

Window:

```text
ece
```

Frequency:

```text
e → 2
c → 1
```

Distinct:

```text
2
```

Valid.

Length:

```text
3
```

Answer:

```text
maxLen = 3
```

---

## Step 4: `right = 3`

Character:

```text
b
```

Window:

```text
eceb
```

Frequency:

```text
e → 2
c → 1
b → 1
```

Distinct:

```text
3
```

But:

```text
k = 2
```

So:

```text
3 > 2
```

Invalid.

---

### Shrink

Remove left character:

```text
e
```

Frequency:

```text
e → 1
c → 1
b → 1
```

Move:

```text
left++
```

Now:

```text
left = 1
```

Window:

```text
ceb
```

Still:

```text
3 distinct
```

Still invalid.

---

### Shrink Again

Remove:

```text
c
```

Frequency:

```text
c → 0
```

Erase `c`.

Now:

```text
e → 1
b → 1
```

Distinct:

```text
2
```

Valid.

Window:

```text
eb
```

Length:

```text
2
```

Maximum remains:

```text
3
```

---

## Step 5: `right = 4`

Character:

```text
a
```

Window:

```text
eba
```

Distinct:

```text
3
```

Invalid.

Shrink.

Remove:

```text
e
```

Now:

```text
ba
```

Distinct:

```text
2
```

Valid.

Length:

```text
2
```

Maximum remains:

```text
3
```

---

# Final Answer

```text
3
```

The longest substring is:

```text
"ece"
```

---

# Interview Tips ⭐

### Tip 1

Whenever you see:

```text
Longest substring

At most K distinct
```

immediately think:

```text
Sliding Window + HashMap
```

---

### Tip 2

The window condition is:

```cpp
freq.size() <= k
```

Invalid condition:

```cpp
freq.size() > k
```

---

### Tip 3

For **longest** valid window:

```cpp
maxLen = max(maxLen, right-left+1);
```

---

### Tip 4

Remember the general structure:

```cpp
for(right...) {

    add s[right];

    while(window is invalid) {

        remove s[left];

        left++;
    }

    update answer;
}
```

This template can solve a huge number of sliding window problems.

---