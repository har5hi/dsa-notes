# LeetCode 1903 - Largest Odd Number in String

---

# Problem Statement

You are given a string `num` representing a large integer.

Return the **largest-valued odd integer (as a string)** that is a **non-empty substring** of `num`.

If no odd integer exists, return an empty string `""`.

A substring is a contiguous sequence of characters within a string.

---

# Intuition

For a number to be **odd**, its **last digit must be odd**.

To obtain the **largest** odd substring:

- We should keep as much of the prefix as possible.
- Therefore, we need the **rightmost odd digit**.
- Everything before (and including) that digit forms the answer.

So, simply scan from the end until an odd digit is found.

---

# Approaches

## 1. Brute Force

### Idea

Generate every possible substring.

For each substring:

- Check whether its last digit is odd.
- Keep the largest valid substring.

---

### Algorithm

1. Generate all substrings.
2. Check if each substring ends with an odd digit.
3. Compare lengths/values.
4. Return the largest odd substring.

---

### Code (C++)

```cpp
class Solution {
public:
    string largestOddNumber(string num) {

        string ans = "";

        for(int i = 0; i < num.size(); i++) {

            for(int j = i; j < num.size(); j++) {

                string sub = num.substr(i, j - i + 1);

                if((sub.back() - '0') % 2 == 1) {

                    if(sub.size() > ans.size() ||
                      (sub.size() == ans.size() && sub > ans))
                        ans = sub;
                }
            }
        }
        return ans;
    }
};
```

---

### Complexity

- **Time:** O(n³)
- **Space:** O(n)

> Since every substring is generated and copied, this approach is very inefficient.

---

## 2. Better Approach (Check Prefixes)

### Idea

Since the answer must begin from index `0` to maximize its value, check every prefix from longest to shortest.

The first prefix ending with an odd digit is the answer.

---

### Algorithm

1. Start from the last character.
2. If digit is odd:
   - Return substring from `0` to current index.
3. Continue moving left.
4. If no odd digit is found, return `""`.

---

### Code

```cpp
class Solution {
public:
    string largestOddNumber(string num) {

        for(int i = num.size() - 1; i >= 0; i--) {
            if((num[i] - '0') % 2 == 1)
                return num.substr(0, i + 1);
        }
        return "";
    }
};
```

---

### Complexity

- **Time:** O(n)
- **Space:** O(n) *(substring creation)*

---

# Interview Tips

### ✅ Observation 1

An odd number is determined **only by its last digit**.
You never need to examine the whole number.

---

### ✅ Observation 2

To maximize the value, remove as few digits from the end as possible.
This naturally leads to scanning from **right to left**.

---

### ✅ Observation 3

No sorting, no dynamic programming, and no extra data structures are required.
A single traversal is sufficient.

---

# Key Takeaway

This problem demonstrates a simple but powerful greedy observation:

> **To maximize an odd number, keep the longest possible prefix whose last digit is odd.**

The solution is:

```text
Traverse from Right

↓

Find Rightmost Odd Digit

↓

Return Prefix Ending There
```

This yields an **O(n)** solution with just a single traversal.