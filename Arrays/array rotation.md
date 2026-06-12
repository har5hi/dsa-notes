# 1. Left Rotate an Array by One Place

## Problem Statement

Given an integer array `nums`, rotate the array to the left by one position.

Example:

```cpp
Input:  [1,2,3,4,5]

Output: [2,3,4,5,1]
```

---

## Approach

The first element will be lost after shifting.

So:

1. Store the first element in a temporary variable.
2. Shift every element one position to the left.
3. Place the stored element at the last index.

--- 

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;

void leftRotateByOne(vector<int>& nums)
{
    int n = nums.size();

    int temp = nums[0];

    for(int i = 0; i < n - 1; i++)
    {
        nums[i] = nums[i + 1];
    }

    nums[n - 1] = temp;
}

int main()
{
    vector<int> nums = {1,2,3,4,5};

    leftRotateByOne(nums);

    for(int x : nums)
    {
        cout << x << " ";
    }

    return 0;
}
```

---

## Time Complexity

* Time Complexity: O(n)
* Space Complexity: O(1)

---

# 2. Right Rotate an Array by K Places

## Problem Statement

Given an array of integers, rotate the array to the right by `k` positions.

Example:

```cpp
Input:
nums = [1,2,3,4,5,6,7]
k = 3

Output:
[5,6,7,1,2,3,4]
```

---

## Approach (Reversal Algorithm)

Instead of shifting elements one by one, we use three reversals.

Steps:

1. Reverse the entire array.
2. Reverse the first `k` elements.
3. Reverse the remaining `n-k` elements.

Example:

```cpp
[1,2,3,4,5,6,7]
```

Reverse whole array:

```cpp
[7,6,5,4,3,2,1]
```

Reverse first k:

```cpp
[5,6,7,4,3,2,1]
```

Reverse remaining:

```cpp
[5,6,7,1,2,3,4]
```

---

## Mistakes I Made While Solving

### 1. Forgetting `k % n`

Example:

```cpp
n = 7
k = 10
```

Rotating by 10 places is equivalent to:

```cpp
10 % 7 = 3
```

Correct:

```cpp
k = k % n;
```

Without this, indices may become invalid.

---

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;

void rotate(vector<int>& nums, int k)
{
    int n = nums.size();

    k = k % n;

    reverse(nums.begin(), nums.end());

    reverse(nums.begin(), nums.begin() + k);

    reverse(nums.begin() + k, nums.end());
}

int main()
{
    vector<int> nums = {1,2,3,4,5,6,7};

    int k = 3;

    rotate(nums, k);

    for(int x : nums)
    {
        cout << x << " ";
    }

    return 0;
}
```

---

## Time Complexity

* Time Complexity: O(n)
* Space Complexity: O(1)

The array is reversed three times, but each element is processed a constant number of times, making the overall complexity O(n).

---