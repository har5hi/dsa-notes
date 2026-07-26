# LeetCode 14 - Longest Common Prefix

---

# Problem Statement

Write a function to find the **longest common prefix** string amongst an array of strings.

If there is no common prefix, return an empty string `""`.

---

# Approaches

## 1. Brute Force (Character-by-Character Comparison)

### Idea

Use the first string as a reference.

For every character in the first string:

- Compare it with the same position in every other string.
- If all strings have the same character, include it in the answer.
- Otherwise, stop immediately.

---

### Algorithm

1. Take the first string.
2. Traverse each character.
3. Compare that character with every other string.
4. If mismatch or string ends:
   - Return answer.
5. Otherwise continue.

---

### Code

```cpp
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {

        if(strs.empty())
            return "";

        string ans = "";

        for(int i = 0; i < strs[0].size(); i++) {
            char ch = strs[0][i];

            for(int j = 1; j < strs.size(); j++) {
                if(i >= strs[j].size() || strs[j][i] != ch)
                    return ans;
            }
            ans += ch;
        }
        return ans;
    }
};
```

---

### Complexity

- **Time:** O(n × m)

Where

- `n` = number of strings
- `m` = length of the shortest string

- **Space:** O(1)

---

## 2. Better Approach (Progressively Shrink Prefix)

### Idea

Assume the first string is the current prefix.

Compare it with every string.

Whenever a mismatch occurs, shorten the prefix until both match.

Eventually, the remaining prefix is common to all strings.

---

### Algorithm

1. Initialize prefix as first string.
2. Compare prefix with every string.
3. While current string doesn't start with prefix:
   - Remove last character of prefix.
4. Return prefix.

---

### Code

```cpp
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {

        string prefix = strs[0];

        for(int i = 1; i < strs.size(); i++) {

            while(strs[i].find(prefix) != 0) {

                prefix.pop_back();

                if(prefix.empty())
                    return "";
            }
        }

        return prefix;
    }
};
```

---

### Complexity

- **Time:** O(n × m)
- **Space:** O(1)

---

## 3. Optimal Approach (Sorting)

### Idea

After sorting the strings lexicographically:

- The most different strings become the **first** and **last** strings.
- Any common prefix shared by all strings must also be shared by these two.

So we only compare:

- First string
- Last string

---

### Example

```text
Input:

["flower","flow","flight"]

After sorting:

["flight","flow","flower"]
```

Compare

```text
flight
flower
```

Common prefix

```text
fl
```

---

### Algorithm

1. Sort the array.
2. Compare first and last strings.
3. Keep matching characters.
4. Return prefix.

---

### Code (Optimal)

```cpp
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {

        sort(strs.begin(), strs.end());

        string first = strs.front();
        string last = strs.back();

        string ans = "";

        for(int i = 0; i < min(first.size(), last.size()); i++) {

            if(first[i] != last[i])
                break;

            ans += first[i];
        }
        return ans;
    }
};
```

---

### Complexity

- **Time:** O(n log n × m)
- **Space:** O(1)

> **Note:** Although elegant, sorting is **not the fastest** because of the `O(n log n)` sorting cost. The character-by-character comparison is generally preferred in interviews due to its linear scan.

---

# Dry Run

Input

```text
["flower","flow","flight"]
```

Take

```text
flower
```

Compare

```text
f ✔

l ✔

o ✖ (flight has 'i')
```

Answer

```text
fl
```

---