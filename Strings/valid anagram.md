# LeetCode 242 - Valid Anagram

---

# Problem Statement

Given two strings `s` and `t`, return **true** if `t` is an **anagram** of `s`, otherwise return **false**.

An **anagram** is a word formed by rearranging the letters of another word using **all the original letters exactly once**.

---

# Intuition

For two strings to be anagrams:

- Both must have the **same length**.
- Every character must appear the **same number of times** in both strings.

Instead of sorting, we can simply count the frequency of each character.
Since the strings contain only lowercase English letters, we need only **26 frequency counters**.

---

# Approaches

## 1. Brute Force (Sorting)

### Idea

If two strings are anagrams, then after sorting them, they become identical.

Example

```text
listen

↓

eilnst
```

```text
silent

↓

eilnst
```

Both become equal.

---

### Algorithm

1. Check if lengths are equal.
2. Sort both strings.
3. Compare them.
4. Return the result.

---

### Code (C++)

```cpp
class Solution {
public:
    bool isAnagram(string s, string t) {

        if(s.size() != t.size())
            return false;

        sort(s.begin(), s.end());
        sort(t.begin(), t.end());

        return s == t;
    }
};
```

---

### Complexity

- **Time:** O(n log n)
- **Space:** O(1) *(Ignoring sorting space)*

---

## 2. Optimal Approach (Frequency Array)

### Idea

Instead of sorting:

- Count every character in `s`.
- Remove every character using `t`.

If at any point a frequency becomes negative,

that means `t` contains more occurrences of a character than `s`.

Hence, they cannot be anagrams.

---

### Algorithm

1. If lengths differ, return false.
2. Create a frequency array of size 26.
3. Count characters of `s`.
4. Decrease frequencies using `t`.
5. If any frequency becomes negative, return false.
6. Otherwise return true.

---

# Code (Optimal)

```cpp
class Solution {
public:
    bool isAnagram(string s, string t) {

        if (s.size() != t.size()) {
            return false;
        }

        vector<int> freq(26, 0);

        for (int i = 0; i < s.size(); i++) {   // Convert character to index: 'a' -> 0, 'b' -> 1,'z' -> 25
            freq[s[i] - 'a']++;
        }

        for (int i = 0; i < t.size(); i++) {

            freq[t[i] - 'a']--;  // Remove one occurrence of the current character.

            if (freq[t[i] - 'a'] < 0) {  // If frequency becomes negative
                return false;
            }
        }
        return true; // If no frequency became negative,
    }
};
```

---

### Complexity

- **Time:** O(n)
- **Space:** O(1)

> The frequency array always has size **26**, so the extra space is constant.

---

# Why Does This Work?

Suppose

```text
s = "aabb"

t = "abab"
```

Count from `s`

```text
a : 2

b : 2
```

Remove using `t`

```text
a : 1

b : 1

a : 0

b : 0
```

Everything becomes zero.

Hence both strings contain exactly the same characters.

---

# Why Check for Negative Frequency?

Consider

```text
s = "abc"

t = "aac"
```

Frequency after counting `s`

```text
a : 1

b : 1

c : 1
```

Traverse `t`

```text
a

↓

0
```

Second

```text
a

↓

-1
```

Negative frequency means

```text
t has more 'a's than s.
```

Therefore,

```text
return false
```

---