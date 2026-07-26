# LeetCode 796 - Rotate String

---

# Problem Statement

Given two strings `s` and `goal`, return **true** if and only if `s` can become `goal` after performing any number of **left rotations**.

A **left rotation** moves the first character of the string to the end.

Example:

```text
abcde

↓

bcdea

↓

cdeab
```

---

# Approaches

## 1. Brute Force (Generate Every Rotation)

### Idea

Generate every possible left rotation of `s`.
After each rotation, compare it with `goal`.
If they become equal, return true.

---

### Algorithm

1. If lengths differ, return false.
2. Rotate the string once.
3. Compare with goal.
4. Repeat `n` times.
5. If no match, return false.

---

### Code (C++)

```cpp
class Solution {
public:
    bool rotateString(string s, string goal) {

        if(s.size() != goal.size())
            return false;

        for(int i = 0; i < s.size(); i++) {

            rotate(s.begin(), s.begin() + 1, s.end());

            if(s == goal)
                return true;
        }

        return false;
    }
};
```

---

### Complexity

- **Time:** O(n²)
- **Space:** O(1)

---

## 2. Better / Optimal Approach (Concatenation Trick)

### Idea

Suppose

```text
s = "abcde"
```

Concatenate it with itself.

```text
abcdeabcde
```

Now look at all substrings of length 5.

```text
abcde

bcdea

cdeab

deabc

eabcd
```

These are exactly all possible rotations.

Therefore,

if

```text
goal
```

exists inside

```text
s + s
```

then it is a valid rotation.

---

### Algorithm

1. If lengths differ, return false.
2. Create

```text
doubled = s + s
```

3. Search whether

```text
goal
```

is a substring of

```text
doubled
```

4. Return the result.

---

# Code (Optimal)

```cpp
class Solution {
public:
    bool rotateString(string s, string goal) {

        // Rotated strings must have equal lengths.
        if(s.size() != goal.size())
            return false;

        string doubled = s + s;

        return doubled.find(goal) != string::npos;
    }
};
```

---

### Complexity

- **Time:** O(n)
- **Space:** O(n)

---

# Understanding `find()`

```cpp
string s = "abcdeabcde";

cout << s.find("cdeab");
```

Output

```text
2
```

Meaning

```text
abcdeabcde
  ↑
```

The substring starts at index **2**.

---

If the substring doesn't exist,

```cpp
s.find("xyz")
```

returns

```cpp
string::npos
```

which means

```text
Not Found
```

Hence,

```cpp
return doubled.find(goal) != string::npos;
```

means

> Return true if `goal` exists inside `doubled`.

---

# Dry Run

Input

```text
s = "abcde"

goal = "cdeab"
```

Length

```text
5 == 5 ✔
```

Create

```text
doubled = "abcdeabcde"
```

Search

```text
abcdeabcde

  cdeab
```

Found ✔

Return

```text
true
```

---

# Interview Tips

### ✅ Observation 1

A rotation never changes the length.
Always check lengths first.

---

### ✅ Observation 2

Remember this interview trick:

```text
Rotation

↓

s + s

↓

Substring Search
```

This pattern appears in multiple string problems.

---

### ✅ Observation 3

Don't waste time generating every rotation.
The concatenation trick is much cleaner.

---

# Alternative Brute Force (Without `rotate()`)

```cpp
class Solution {
public:
    bool rotateString(string s, string goal) {

        if(s.size() != goal.size())
            return false;

        for(int i = 0; i < s.size(); i++) {

            // Move the first character to the end.
            s = s.substr(1) + s[0];

            if(s == goal)
                return true;
        }

        return false;
    }
};
```

### Complexity

- **Time:** O(n²)
- **Space:** O(n)

---