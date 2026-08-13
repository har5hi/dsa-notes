# LeetCode 76 - Minimum Window Substring

---

# Problem Statement

Given two strings

- `s`
- `t`

Return the **minimum window substring** of `s` such that **every character of `t` (including duplicates)** is present in the window.

If no such window exists,

return

```text
""
```

If multiple answers exist,

return any one of them.

---

# Example 1

```text
Input

s = "ADOBECODEBANC"

t = "ABC"
```

Output

```text
"BANC"
```

Explanation

"BANC" is the smallest substring containing

```
A

B

C
```

---

# Approaches

---

# 1. Brute Force

## Intuition

Generate every possible substring.

For each substring, count character frequencies.

Check whether it contains all characters of `t`.

Keep the smallest valid substring.

---

# Algorithm

1. Generate every substring.
2. Count frequencies.
3. Compare with `t`.
4. If valid,
   update minimum length.
5. Return smallest substring.

---

# Brute Force Code

```cpp
class Solution {
public:

    string minWindow(string s, string t) {

        int n = s.size();

        string ans = "";

        int minLen = INT_MAX;

        for(int i = 0; i < n; i++) {
            vector<int> freq(128,0);

            for(int j = i; j < n; j++) {
                freq[s[j]]++;

                bool ok = true;

                vector<int> need(128,0);

                for(char ch : t)
                    need[ch]++;

                for(int k = 0; k < 128; k++) {

                    if(freq[k] < need[k]) {

                        ok = false;
                        break;
                    }
                }

                if(ok) {

                    if(j-i+1 < minLen) {
                        minLen = j-i+1;
                        ans = s.substr(i,minLen);
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

```text
O(n³)
```

Generating every substring

+

checking frequencies.

---

### Space

```text
O(1)
```

---

# 2. Better Approach

Precompute frequency of `t`.

For every starting index, expand until all required characters are found.

Still

```
O(n²)
```

Not efficient enough.

---

# 3. Optimal Approach (Sliding Window)

---

# Intuition

Maintain a sliding window.

Expand the window until it becomes

```text
VALID
```

Once valid, shrink it from the left

to make it as small as possible.

While shrinking, keep updating the answer.

This is different from previous problems.

Earlier, we wanted

```
Largest Window
```

or

```
Count of Windows
```

Here,

we want

```
Smallest Valid Window
```

---

# Key Idea

The window has two states.

### Invalid

```
A D O

Missing

B

C
```

Expand.

---

### Valid

```
A D O B E C
```

Contains

```
A

B

C
```

Now,

instead of expanding,

start shrinking.

---

# Algorithm

1. Store frequency of every character in `t`.
2. Expand the window using `right`.
3. Update current window frequencies.
4. If the window becomes valid,
   try shrinking from the left.
5. Keep updating the smallest valid window.
6. Return the answer.

---

# Optimal Code (Fully Commented)

```cpp
class Solution {
public:

    string minWindow(string s, string t) {

        vector<int> need(128,0);

        // Store frequency of characters required
        for(char ch : t)
            need[ch]++;

        int left = 0;
        int count = 0;
        int start = 0;
        int minLen = INT_MAX;

        for(int right = 0; right < s.size(); right++) {

            // Include current character
            if(need[s[right]] > 0)
                count++;

            need[s[right]]--;

            // Window contains every required character
            while(count == t.size()) {

                // Update minimum window
                if(right-left+1 < minLen) {

                    minLen = right-left+1;
                    start = left;
                }

                // Remove left character
                need[s[left]]++;

                if(need[s[left]] > 0)
                    count--;

                left++;
            }
        }

        if(minLen == INT_MAX)
            return "";

        return s.substr(start,minLen);
    }
};
```

---

# Complexity

### Time

```text
O(n)
```

Every character

- enters the window once
- leaves the window once

---

### Space

```text
O(1)
```

Only a frequency array of size

```
128
```

is used.

---

# Why Sliding Window Works

The window expands until it becomes valid.

Once valid, expanding further only makes it larger.

Since we need the **smallest** valid window,

we immediately start shrinking it.

This process continues until the window becomes invalid again.

Thus, both pointers move only forward,

giving an overall complexity of

```text
O(n)
```

---

# Frequently Asked Interview Questions

---

## Q1. Why don't we use a HashSet?

Because duplicates matter.

```
AABC
```

and

```
ABC
```

have the same set,

but different frequencies.

---

## Q2. Why use a Frequency Array instead of a HashMap?

Characters are ASCII.

ASCII contains only

```
128
```

characters.

Array lookup

```
O(1)
```

is faster than HashMap lookup.

---

## Q3. Why is

```cpp
need[s[right]]--;
```

done even when the character isn't required?

Suppose

```
Need

A = 0
```

Another

```
A
```

comes.

Need becomes

```
-1
```

Negative simply means

```
Extra copies available.
```

Later,

while shrinking,

these extra copies can be removed without making the window invalid.

---

## Q4. Why do we shrink immediately after finding a valid window?

Because the goal is

```text
Minimum Window
```

A larger valid window is never better than a smaller valid window.

So as soon as the window becomes valid,

we try to make it smaller.

---

## Q5. Why does this algorithm run in O(n)?

Because

```
Left pointer

moves only forward

Right pointer

moves only forward
```

Each character

- enters once
- leaves once

Maximum operations

```
2n
```

Therefore

```
O(n)
```

---

# Comparison with Other Sliding Window Problems

## LC 3 — Longest Substring Without Repeating Characters

Goal

```text
Largest Valid Window
```

Update

```cpp
maxLen = max(maxLen, right-left+1);
```

---

## LC 424 — Longest Repeating Character Replacement

Goal

```text
Largest Valid Window
```

Update

```cpp
maxLen = max(maxLen, right-left+1);
```

---

## LC 992 — Subarrays With K Different Integers

Goal

```text
Count Windows
```

Update

```cpp
ans += right-left+1;
```

---

## LC 1358 — Number of Substrings Containing ABC

Goal

```text
Count Windows
```

Update

```cpp
ans += n-right;
```

---

## LC 76 — Minimum Window Substring

Goal

```text
Smallest Valid Window
```

Update

```cpp
if(right-left+1 < minLen)
{
    minLen = right-left+1;
    start = left;
}
```

This is a completely different sliding window pattern.

---

# Sliding Window Pattern Summary

## Pattern 1

Longest Valid Window

```cpp
maxLen = max(maxLen, right-left+1);
```

---

## Pattern 2

Count Valid Windows

```cpp
ans += right-left+1;
```

---

## Pattern 3

Count Remaining Windows

```cpp
ans += n-right;
```

---

## Pattern 4

Minimum Valid Window

```cpp
if(window is valid)
{
    update minimum

    shrink
}
```

---

## Pattern 5

Fixed Size Sliding Window

Window size never changes.

---

# Final Cheat Sheet

| Pattern | Goal | Update Statement | Example Problems |
|---------|------|------------------|------------------|
| Longest Valid Window | Maximum length | `maxLen = max(maxLen, right-left+1)` | LC 3, 424, 904, 1004 |
| Count Valid Windows | Count subarrays/substrings | `ans += right-left+1` | LC 930, 1248, 992 |
| Count Remaining Windows | Count using current left | `ans += n-right` | LC 1358 |
| Minimum Valid Window | Smallest substring | Update answer, then shrink | **LC 76** |
| Fixed Size Window | Window length fixed | Slide by adding & removing one element | LC 1423, LC 643 |

---