# LeetCode 3 - Longest Substring Without Repeating Characters

---

# Problem Statement

Given a string `s`, find the **length of the longest substring** without repeating characters.
A **substring** is a contiguous sequence of characters within a string.

---

# Approaches

---

# 1. Brute Force

---

# Algorithm

1. Generate every substring.
2. Store characters in a Hash Set.
3. If a duplicate appears,
   stop checking that substring.
4. Update maximum length.
5. Return answer.

---

# Brute Force Code

```cpp
class Solution {
public:

    int lengthOfLongestSubstring(string s) {

        int maxLen = 0;

        // Choose every starting index
        for(int i = 0; i < s.length(); i++) {
            unordered_set<char> st;

            // Extend substring
            for(int j = i; j < s.length(); j++) {

                // Duplicate found
                if(st.find(s[j]) != st.end())
                    break;

                st.insert(s[j]);
                maxLen = max(maxLen, j - i + 1);
            }
        }
        return maxLen;
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
O(n)
```

---

# 2. Optimal Approach (Sliding Window + Two Pointers)

---

# Intuition

Instead of generating every substring,

maintain one window.

Initially

```text
abc
```

Everything is unique.

Now add

```text
a
```

Window becomes

```text
abca
```

Duplicate found.

Instead of restarting,

remove characters from the left.

```
abca

↓

bca
```

Now duplicate disappears.

Continue expanding.

This way,

each character is processed only once.

---

# Algorithm

1. Create an empty Hash Set.
2. Keep two pointers (`left` and `right`).
3. Expand the window using `right`.
4. If duplicate appears,
   remove characters from the left until it becomes unique.
5. Insert current character.
6. Update maximum length.
7. Return answer.

---

# Optimal Code (Fully Commented)

```cpp
class Solution {
public:

    int lengthOfLongestSubstring(string s) {

        // Stores all unique characters
        // currently inside the window.
        unordered_set<char> st;

        // Left pointer of the window.
        int left = 0;

        // Stores maximum length found.
        int maxLen = 0;

        // Expand the window.
        for(int right = 0; right < s.length(); right++) {

            // If duplicate exists,
            // shrink the window.
            while(st.find(s[right]) != st.end()) {

                // Remove leftmost character.
                st.erase(s[left]);

                // Move left pointer.
                left++;
            }
            // Add current character.
            st.insert(s[right]);

            // Update longest valid window.
            maxLen = max(maxLen, right - left + 1);
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
O(n)
```

---