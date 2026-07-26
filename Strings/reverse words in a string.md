# LeetCode 151 - Reverse Words in a String

---

# Problem Statement

Given an input string `s`, reverse the order of the words.

A **word** is defined as a sequence of non-space characters.

The returned string should:

- Have words in reverse order.
- Contain only a single space between words.
- Have no leading or trailing spaces.

### Example 1

```text
Input: s = "the sky is blue"
Output: "blue is sky the"
```

---

# Intuition

The main challenge is **handling extra spaces**.

We need to:

1. Ignore leading spaces.
2. Ignore trailing spaces.
3. Ignore multiple spaces between words.
4. Reverse the order of the words.

Instead of reversing the entire string, we can simply extract each word and then rebuild the answer in reverse order.

---

# Approaches

## 1. Brute Force (Store Words in Vector)

### Idea

- Traverse the string.
- Extract every word.
- Store them in a vector.
- Traverse the vector backwards to build the answer.

---

### Algorithm

1. Traverse the string.
2. Skip all spaces.
3. Extract one complete word.
4. Store it in a vector.
5. Traverse the vector from back to front.
6. Join words using one space.

---

### Code (C++)

```cpp
class Solution {
public:
    string reverseWords(string s) {
        vector<string> words;
        int n = s.size();
        int i = 0;

        while(i < n) {

            while(i < n && s[i] == ' ')
                i++;

            string word = "";

            while(i < n && s[i] != ' ') {
                word += s[i];
                i++;
            }

            if(!word.empty())
                words.push_back(word);
        }

        string ans = "";

        for(int i = words.size() - 1; i >= 0; i--) {
            ans += words[i];
            if(i != 0)
                ans += " ";
        }

        return ans;
    }
};
```

---

### Complexity

- **Time:** O(n)
- **Space:** O(n)

---

## 2. Better Approach (Build Answer While Traversing)

### Idea

Instead of storing every word in a vector, prepend each new word to the answer.

For every extracted word:

- If answer is empty

```text
ans = word
```

Else

```text
ans = word + " " + ans
```

Thus, words automatically appear in reverse order.

---

### Algorithm

1. Traverse string.
2. Skip extra spaces.
3. Extract one word.
4. Insert it before the existing answer.
5. Return answer.

---

### Code

```cpp
class Solution {
public:
    string reverseWords(string s) {
        int i = 0;
        int n = s.size();
        string ans = "";

        while(i < n) {

            while(i < n && s[i] == ' ')
                i++;

            string word = "";

            while(i < n && s[i] != ' ') {
                word += s[i];
                i++;
            }

            if(!word.empty()) {
                if(ans.empty())
                    ans = word;
                else
                    ans = word + " " + ans;
            }
        }
        return ans;
    }
};
```

---

### Complexity

- **Time:** O(n²) *(string prepending copies the existing answer every time)*
- **Space:** O(n)

> **Note:** Although it avoids a vector, repeatedly inserting at the front of a string makes this approach slower.

---

## 3. Optimal Approach (Reverse + Reverse Each Word)

### Idea

This is the classic in-place solution.

Steps:

1. Remove extra spaces.
2. Reverse the entire string.
3. Reverse each individual word.

Example:

```text
Input:
the sky is blue

After full reverse:
eulb si yks eht

Reverse each word:
blue is sky the
```

---

### Algorithm

1. Remove extra spaces.
2. Reverse complete string.
3. Traverse string.
4. Reverse every word separately.
5. Return answer.

---

### Code (C++)

```cpp
class Solution {
public:
    string reverseWords(string s) {
        int n = s.length();
        string ans = "";

        reverse(s.begin(), s.end());

        for(int i = 0; i<n; i++){
            string word = "";
            while(i<n && s[i] != ' '){
                word += s[i];
                i++;
            }
            reverse(word.begin(), word.end());
            if(word.length() > 0){
                ans += " " + word; 
            }
        }
        return ans.substr(1);
    }
};
```

---

### Complexity

- **Time:** O(n)
- **Space:** O(n)

> If modifying the input string directly is allowed, this approach can be implemented with **O(1)** extra space.

---

# Key Takeaway

This problem teaches an important interview pattern:

> **Clean the string first, then perform the required transformation.**

For reversing words, the standard optimal strategy is:

```text
Remove Extra Spaces

↓

Reverse Entire String

↓

Reverse Each Word
```

This gives an **O(n)** solution and is the approach most interviewers expect.