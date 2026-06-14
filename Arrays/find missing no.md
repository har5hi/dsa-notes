# LeetCode 268 - Missing Number

## Problem Statement

Given an array `nums` containing `n` distinct numbers in the range `[1, n + 1]`, return the only number that is missing from the array.

---

## Approach

The array contains numbers from `1` to `n + 1`, with exactly one number missing.

We know the sum of the first `N` natural numbers is:

```text
N * (N + 1) / 2
```

Since the array size is `n`, the numbers should actually range from `1` to `n + 1`.

---

## Dry Run

```text
arr = [1, 2, 4, 5]

n = 4

Expected Sum = (n + 1) * (n + 2) / 2
             = 5 * 6 / 2
             = 15

Actual Sum = 1 + 2 + 4 + 5
           = 12

Missing Number = 15 - 12
               = 3
```

---

## Mistakes I Made While Solving

### 1. Using `n * (n + 1) / 2`

Initially, I wrote:

```cpp
int sum = n * (n + 1) / 2;
```

This is incorrect because `n` represents the size of the array, not the largest number.

For:

```cpp
arr = {1, 2, 4, 5}
```

```text
Array Size = 4
Numbers should be = 1, 2, 3, 4, 5
```

The correct sum should be:

```cpp
(n + 1) * (n + 2) / 2
```

---

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;

int findMissing(int arr[], int n)
{
    int expectedSum = (n + 1) * (n + 2) / 2;

    int actualSum = 0;

    for(int i = 0; i < n; i++)
    {
        actualSum += arr[i];
    }

    return expectedSum - actualSum;
}

int main()
{
    int arr[] = {1, 2, 4, 5};

    int n = sizeof(arr) / sizeof(arr[0]);

    int result = findMissing(arr, n);

    cout << "The Missing Number is: " << result << endl;

    return 0;
}
```

---

## Time Complexity

* Time Complexity: O(n)
* Space Complexity: O(1)

The array is traversed only once and no extra space is used.

---

## Optimal Approach (XOR)

### Intuition

The XOR operator has two important properties:

```cpp
a ^ a = 0
a ^ 0 = a
```

This means that if we XOR a number with itself, it gets cancelled out.

Since the array contains numbers from `0` to `n` with exactly one number missing:

* XOR all elements of the array.
* XOR all numbers from `1` to `n`.
* Every number that appears in both places cancels out.
* The missing number is left behind.

---

## Dry Run

```text
nums = [3, 0, 1]

n = 3

xor1 = XOR of array elements
xor2 = XOR of numbers from 1 to n

xor1 = 3 ^ 0 ^ 1 = 2

xor2 = 1 ^ 2 ^ 3 = 0

Answer = xor1 ^ xor2
       = 2 ^ 0
       = 2
```

Missing number = 2

---

## Why Does It Work?

Consider:

```text
nums = [3, 0, 1]

Numbers should be:

0, 1, 2, 3
```

XOR of all numbers:

```text
(3 ^ 0 ^ 1) ^ (1 ^ 2 ^ 3)
```

Rearranging:

```text
(1 ^ 1) ^ (3 ^ 3) ^ 0 ^ 2
```

Cancelling pairs:

```text
0 ^ 0 ^ 0 ^ 2
```

Result:

```text
2
```

The missing number remains.

---

## Code (Optimal XOR Solution)

```cpp
class Solution {
public:
    int missingNumber(vector<int>& nums) {

        int xor1 = 0;
        int xor2 = 0;

        int n = nums.size();

        for(int i = 0; i < n; i++) {
            xor1 ^= nums[i];
            xor2 ^= (i + 1);
        }

        return xor1 ^ xor2;
    }
};
```

---

## Time Complexity

* Time Complexity: O(n)
* Space Complexity: O(1)

The array is traversed only once and no extra space is used.

---

## Comparison of Approaches

| Approach | Time | Space |
|-----------|---------|---------|
| Brute Force (Linear Search) | O(n²) | O(1) |
| Sum Formula | O(n) | O(1) |
| XOR | O(n) | O(1) |

---

## Key Takeaway

Whenever:

* Numbers are from `0` to `n`
* Exactly one number is missing
* O(1) space is required

Think about XOR because:

```cpp
a ^ a = 0
```

allows all matching numbers to cancel out, leaving only the missing number.