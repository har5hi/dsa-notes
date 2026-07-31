# LeetCode 424 - Longest Repeating Character Replacement

---

# Problem Statement

You are given a string `s` consisting of only uppercase English letters and an integer `k`.

You can choose **at most `k` characters** of the string and change them to any other uppercase English character.

Return the **length of the longest substring** containing the **same letter** after performing at most `k` replacements.

---

# Example 1

```text
Input

s = "ABAB"

k = 2
```

Output

```text
4
```

Explanation

Replace

```text
A → B
```

or

```text
B → A
```

Entire string becomes

```text
AAAA
```

or

```text
BBBB
```

Longest length

```text
4
```

---

# Key Observation

Inside any window,

we don't care **which character** we replace.

We only care

```text
How many characters

are NOT

the most frequent character.
```

Example

Window

```text
A A B A C
```

Frequency

```text
A → 3

B → 1

C → 1
```

Most frequent character

```text
A
```

Frequency

```text
3
```

Window size

```text
5
```

Characters to replace

```text
5 - 3

=

2
```

Replace

```text
B

C
```

↓

```text
A A A A A
```

Done.

---

# Important Formula

Suppose

Window size

```text
windowSize
```

Most frequent character

```text
maxFreq
```

Characters that must be replaced

```text
windowSize - maxFreq
```

If

```text
windowSize - maxFreq ≤ k
```

the window is valid.

Otherwise,

shrink it.

This is the heart of the problem.

---

# Approaches

---

# 1. Brute Force

## Intuition

Generate every possible substring.
For every substring, count frequency of each character.

Find

```text
Maximum Frequency
```

Calculate

```text
Window Size - Maximum Frequency
```

If replacements required

```text
≤ k
```

update answer.

---

# Algorithm

1. Generate every substring.
2. Count character frequencies.
3. Find maximum frequency.
4. Calculate replacements needed.
5. Update answer.
6. Return maximum length.

---

# Brute Force Code

```cpp
class Solution {
public:

    int characterReplacement(string s, int k) {

        int n = s.length();
        int ans = 0;

        for(int i = 0; i < n; i++) {
            vector<int> freq(26,0);

            int maxFreq = 0;

            for(int j = i; j < n; j++) {
                freq[s[j]-'A']++;

                maxFreq = max(maxFreq, freq[s[j]-'A']);

                int windowSize = j - i + 1;
                int replacements = windowSize - maxFreq;

                if(replacements <= k)
                    ans = max(ans, windowSize);
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
O(26)

≈

O(1)
```

---

# 2. Optimal Approach (Sliding Window)

---

# Intuition

Maintain one sliding window.

Expand the window using the

```text
right
```

pointer.

Store frequency of every character.

Keep track of

```text
Maximum Frequency
```

inside the current window.

If

```text
windowSize - maxFreq ≤ k
```

window is valid.

Otherwise,

move

```text
left
```

until it becomes valid again.

Keep updating the maximum length.

---

# Algorithm

1. Create a frequency array.
2. Expand using the right pointer.
3. Update frequency.
4. Update maximum frequency.
5. Calculate replacements required.
6. If replacements exceed `k`,
   shrink the window.
7. Update answer.

---

# Optimal Code (Fully Commented)

```cpp
class Solution {
public:

    int characterReplacement(string s, int k) {

        // Frequency of each character
        vector<int> freq(26,0);

        // Left pointer
        int left = 0;

        // Maximum frequency inside current window
        int maxFreq = 0;

        // Maximum valid window length
        int maxLen = 0;

        // Expand window
        for(int right = 0; right < s.length(); right++) {

            // Add current character
            freq[s[right]-'A']++;

            // Update highest frequency
            maxFreq = max(maxFreq, freq[s[right]-'A']);

            // Current window size
            int windowSize = right - left + 1;

            // Too many replacements required
            while(windowSize - maxFreq > k) {

                freq[s[left]-'A']--;

                left++;

                windowSize = right - left + 1;
            }

            // Window is valid
            maxLen = max(maxLen, windowSize);
        }
        return maxLen;
    }
};
```

---

# Complexity

Time

```text
O(n)
```

Space

```text
O(26)

≈

O(1)
```

---