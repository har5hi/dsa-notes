# LeetCode 205 - Isomorphic Strings

---

# Problem Statement

Given two strings `s` and `t`, determine if they are **isomorphic**.

Two strings are isomorphic if:

- Every character in `s` can be replaced by exactly one character in `t`.
- No two different characters in `s` can map to the same character.
- A character may map to itself.

---

# Intuition

The important observation is:

A mapping must work **both ways**.

If

```text
a → x
```

then

```text
x → a
```

must also be true.

Otherwise,

```text
a → x

b → x
```

would incorrectly be considered valid.

Therefore we maintain:

- one map from `s → t`
- another map from `t → s`

---

# Approaches

## 1. Brute Force

### Idea

For every character, search through all previous characters to check whether the mapping is consistent.

---

### Algorithm

1. For each index `i`
2. Compare with every previous index `j`
3. If characters in `s` are same, characters in `t` must also be same.
4. If characters in `t` are same, characters in `s` must also be same.
5. Otherwise return false.

---

### Code (C++)

```cpp
class Solution {
public:
    bool isIsomorphic(string s, string t) {

        int n = s.size();

        for(int i = 0; i < n; i++) {

            for(int j = i + 1; j < n; j++) {

                if((s[i] == s[j] && t[i] != t[j]) ||
                   (s[i] != s[j] && t[i] == t[j]))
                    return false;
            }
        }

        return true;
    }
};
```

---

### Complexity

- **Time:** O(n²)
- **Space:** O(1)

---

## 2. Optimal Approach (Two Hash Maps)

### Idea

Store mappings in both directions.

Example

```text
egg

↓

add
```

Store

```text
e → a

g → d
```

Also store

```text
a → e

d → g
```

Whenever we encounter an existing character, simply verify its mapping.

---

### Algorithm

1. Create two hash maps.
2. Traverse both strings together.
3. If mapping already exists:
   - Verify it.
4. Otherwise create new mapping.
5. Do this in both directions.
6. If every mapping is valid, return true.

---

## Code (Optimal)

```cpp
class Solution {
public:
    bool isIsomorphic(string s, string t) {

        // Stores mapping from characters of s to characters of t
        unordered_map<char, char> mapST;

        // Stores reverse mapping from t to s
        unordered_map<char, char> mapTS;

        // Traverse both strings simultaneously
        for(int i = 0; i < s.length(); i++) {

            // Current characters from both strings
            char chS = s[i];
            char chT = t[i];

            // If character from s was already mapped before
            if(mapST.count(chS)) {

                // Existing mapping should match current character in t
                if(mapST[chS] != chT)
                    return false;
            }

            // Otherwise create a new mapping
            else {
                mapST[chS] = chT;
            }

            // Check reverse mapping

            // If character from t already has a mapping
            if(mapTS.count(chT)) {

                // It must map back to the same character in s
                if(mapTS[chT] != chS)
                    return false;
            }

            // Otherwise create reverse mapping
            else {
                mapTS[chT] = chS;
            }
        }

        // All mappings are valid
        return true;
    }
};
```

---

### Complexity

- **Time:** O(n)
- **Space:** O(n)

---

# Dry Run

Input

```text
s = "paper"

t = "title"
```

Initially

```text
mapST = {}

mapTS = {}
```

---

### i = 0

```text
p → t
```

Store

```text
mapST

p → t
```

```text
mapTS

t → p
```

---

### i = 1

```text
a → i
```

Store

```text
a → i

i → a
```

---

### i = 2

```text
p → t
```

Already exists

```text
p → t ✔
```

Continue.

---

### i = 3

```text
e → l
```

Store

```text
e → l

l → e
```

---

### i = 4

```text
r → e
```

Store

```text
r → e

e → r
```

No conflicts.

Answer

```text
true
```

---

# Why Two Maps?

Suppose

```text
ab

cc
```

Using only

```text
a → c

b → c
```

Everything appears valid.

But this is incorrect because

```text
a

and

b
```

cannot both map to

```text
c
```

The second map immediately catches this.

```text
c → a
```

Later

```text
c → b
```

Conflict found.

Return

```text
false
```

---

# Alternative Optimal Approach (Using Arrays)

Since characters are ASCII (256 possible values), we can replace hash maps with arrays.

### Code

```cpp
class Solution {
public:
    bool isIsomorphic(string s, string t) {

        vector<int> mapST(256, -1);
        vector<int> mapTS(256, -1);

        for(int i = 0; i < s.size(); i++) {

            // If one character has been seen before
            // but the other hasn't, mapping is inconsistent
            if(mapST[s[i]] != mapTS[t[i]])
                return false;

            // Store the current index (+1 because default value is -1)
            mapST[s[i]] = i;
            mapTS[t[i]] = i;
        }

        return true;
    }
};
```

### Complexity

- **Time:** O(n)
- **Space:** O(1)

> Since the array size is fixed (256), the extra space is considered constant.

---