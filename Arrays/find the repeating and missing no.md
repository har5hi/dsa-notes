# Find the Repeating and Missing Number

## Problem Statement

Given an integer array `nums` of size `n` containing numbers from `1` to `n`.

* One number `A` appears **twice** (Repeating Number).
* One number `B` is **missing**.

Return `[A, B]`, where:

* `A` → Repeating Number
* `B` → Missing Number

**Constraint:** You are **not allowed to modify the original array**.

---

# Approach 1: Brute Force

## Idea

For every number from `1` to `n`, count its frequency in the array.

* Frequency = 2 → Repeating number.
* Frequency = 0 → Missing number.

---

## Algorithm

1. Iterate from `1` to `n`.
2. For each number, traverse the entire array and count occurrences.
3. If count is `2`, store it as repeating.
4. If count is `0`, store it as missing.

---

## Code

```cpp
class Solution {
public:
    vector<int> findMissingRepeatingNumbers(vector<int> nums) {
        int n = nums.size();

        int repeating = -1;
        int missing = -1;

        for(int i = 1; i <= n; i++) {

            int cnt = 0;

            for(int j = 0; j < n; j++) {
                if(nums[j] == i)
                    cnt++;
            }

            if(cnt == 2)
                repeating = i;

            else if(cnt == 0)
                missing = i;

            if(repeating != -1 && missing != -1)
                break;
        }

        return {repeating, missing};
    }
};
```

---

## Dry Run

```text
nums = [1, 2, 2, 4]

i = 1 → count = 1
i = 2 → count = 2 → repeating = 2
i = 3 → count = 0 → missing = 3
i = 4 → count = 1

Answer = [2, 3]
```

---

## Complexity Analysis

### Time Complexity

For every number (1 to n), we scan the entire array.

```text
O(n × n) = O(n²)
```

### Space Complexity

```text
O(1)
```

---

# Approach 2: Better Solution (Hashing)

## Idea

Store frequencies of all numbers.

Since numbers are from `1` to `n`, create a frequency array of size `n+1`.

After counting:

* Frequency = 2 → Repeating
* Frequency = 0 → Missing

---

## Why It Works

The frequency array directly tells how many times each number appears.

Example:

```text
nums = [1,2,2,4]

freq:
0 1 2 3 4
0 1 2 0 1

2 occurs twice
3 occurs zero times
```

Hence:

```text
Repeating = 2
Missing = 3
```

---

## Code

```cpp
class Solution {
public:
    vector<int> findMissingRepeatingNumbers(vector<int> nums) {

        int n = nums.size();

        vector<int> freq(n + 1, 0);

        for(int i = 0; i < n; i++) {
            freq[nums[i]]++;
        }

        int repeating = -1;
        int missing = -1;

        for(int i = 1; i <= n; i++) {

            if(freq[i] == 2)
                repeating = i;

            else if(freq[i] == 0)
                missing = i;
        }

        return {repeating, missing};
    }
};
```

---

## Complexity Analysis

### Time Complexity

```text
Building frequency array : O(n)
Finding missing/repeating : O(n)

Total = O(2n)
      = O(n)
```

### Space Complexity

```text
Frequency Array = O(n)
```

---

# Approach 3: Optimal Solution (Mathematics)

## Observation

Let:

```text
A = Repeating Number
B = Missing Number
```

For numbers from `1` to `n`:

### Expected Sum

```text
S = n(n+1)/2
```

### Expected Square Sum

```text
P = n(n+1)(2n+1)/6
```

Now calculate actual values from array:

```text
actualSum
actualSquareSum
```

---

## Equation 1

Difference of sums:

```text
actualSum - S
```

Since A appears twice and B is missing:

```text
(A - B)
```

Let

x = A - B

```text
x = actualSum - S
```

---

## Equation 2

Difference of square sums:

```text
actualSquareSum - P
```

becomes

```text
A² - B²
```

Using identity:

```text
A² - B² = (A-B)(A+B)
```

Therefore:

```text
actualSquareSum - P
= x(A+B)
```

Let

```text
y = (actualSquareSum - P) / x
```

Then:

```text
y = A + B
```

---

## Solving Equations

We now have:

```text
A - B = x
A + B = y
```

Add both:

```text
2A = x + y

A = (x+y)/2
```

Then:

```text
B = y - A
```

---

## Example

```text
nums = [1,2,2,4]

Expected Sum:
1+2+3+4 = 10

Actual Sum:
1+2+2+4 = 9

x = 9 - 10
  = -1

Expected Square Sum:
1+4+9+16 = 30

Actual Square Sum:
1+4+4+16 = 25

25 - 30 = -5

y = (-5)/(-1)
  = 5

A = (x+y)/2
  = (-1+5)/2
  = 2

B = y - A
  = 5 - 2
  = 3
```

Answer:

```text
[2,3]
```

---

## Code

```cpp
class Solution {
public:
    vector<int> findMissingRepeatingNumbers(vector<int> nums) {

        long long n = nums.size();

        long long S = (n * (n + 1)) / 2;
        long long P = (n * (n + 1) * (2 * n + 1)) / 6;

        long long actualSum = 0;
        long long actualSquareSum = 0;

        for(int num : nums) {
            actualSum += num;
            actualSquareSum += 1LL * num * num;
        }

        long long x = actualSum - S;              // A - B

        long long y =
            (actualSquareSum - P) / x;            // A + B

        long long A = (x + y) / 2;               // Repeating
        long long B = y - A;                     // Missing

        return {(int)A, (int)B};
    }
};
```

---

# Complexity Comparison

| Approach             | Time  | Space |
| -------------------- | ----- | ----- |
| Brute Force          | O(n²) | O(1)  |
| Hashing              | O(n)  | O(n)  |
| Mathematical Optimal | O(n)  | O(1)  |

---