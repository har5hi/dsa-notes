# Valid Anagram

## Intuition

Two strings are anagrams if: they contain the same characters, with the same frequency

Instead of sorting, we use a frequency array of size 26 for lowercase English letters.

Increase count for characters in s

Decrease count for characters in t

If any count becomes negative → extra character exists in t → not an anagram

## Important Concept
Frequency Array / Hashing

```cpp vector<int> freq(26, 0);

Index 0 → 'a'

Index 1 → 'b'

...

Index 25 → 'z'

Character mapping:

s[i] - 'a'

Example:

'c' - 'a' = 2
```
## Code:

```cpp 
class Solution {
public:
    bool isAnagram(string s, string t) {
        if (s.size() != t.size()){
            return false;
        }
        vector<int> freq(26, 0);

        for (int i = 0; i < s.size(); i++) {
            freq[s[i] - 'a']++;
        }

        for (int i = 0; i < t.size(); i++) {
            freq[t[i] - 'a']--;
            if (freq[t[i] - 'a'] < 0) {
                return false;
            }
        }
        return true;
    }
};
```

## Time Complexity

- Two loops over n characters: O(n)

## Space Complexity

- Frequency array size is always 26: O(1)
