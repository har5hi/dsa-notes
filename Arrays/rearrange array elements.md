# Rearrange Array Elements by Sign

LeetCode 2149 - Rearrange Array Elements by Sign

---

# Problem 1: Equal Number of Positive and Negative Elements

## Idea

Since the number of positive and negative elements is equal:

- Positive numbers must be placed at even indices.
- Negative numbers must be placed at odd indices.
- Maintain two pointers:
  - `pos = 0`
  - `neg = 1`

Whenever we encounter:

- Positive → place at `pos`, then `pos += 2`
- Negative → place at `neg`, then `neg += 2`

---

## Code

```cpp
class Solution {
public:
    vector<int> rearrangeArray(vector<int>& nums) {

        int n = nums.size();

        vector<int> ans(n);

        int pos = 0;
        int neg = 1;

        for(int i = 0; i < n; i++) {

            if(nums[i] < 0) {
                ans[neg] = nums[i];
                neg += 2;
            }
            else {
                ans[pos] = nums[i];
                pos += 2;
            }
        }

        return ans;
    }
};
```

---

## Complexity

### Time

```text
O(N)
```

### Space

```text
O(N)
```

---

# Problem 2: Positive and Negative Counts Are Not Equal

Example:

```cpp
[1,2,3,-1,-2]
```

Positives = 3

Negatives = 2

After alternating:

```cpp
[1,-1,2,-2,3]
```

Remaining positive elements are appended at the end.

---

## Approach

### Step 1

Store positives separately.

```cpp
pos = [1,2,3]
```

### Step 2

Store negatives separately.

```cpp
neg = [-1,-2]
```

### Step 3

Fill answer alternately.

```cpp
ans = [1,-1,2,-2,_]
```

### Step 4

Append remaining elements.

```cpp
ans = [1,-1,2,-2,3]
```

---

## Code

```cpp
vector<int> altNumbers(vector<int>& arr, int n) {

    vector<int> pos;
    vector<int> neg;

    for(int i = 0; i < n; i++) {

        if(arr[i] >= 0)
            pos.push_back(arr[i]);
        else
            neg.push_back(arr[i]);
    }

    vector<int> ans(n);

    if(pos.size() > neg.size()) {

        for(int i = 0; i < neg.size(); i++) {

            ans[2*i] = pos[i];
            ans[2*i + 1] = neg[i];
        }

        int index = neg.size() * 2;

        for(int i = neg.size(); i < pos.size(); i++)
            ans[index++] = pos[i];
    }
    else {

        for(int i = 0; i < pos.size(); i++) {

            ans[2*i] = pos[i];
            ans[2*i + 1] = neg[i];
        }

        int index = pos.size() * 2;

        for(int i = pos.size(); i < neg.size(); i++)
            ans[index++] = neg[i];
    }

    return ans;
}
```

---

## Complexity

### Time

```text
O(N)
```

- One pass to separate.
- One pass to build answer.

### Space

```text
O(N)
```

Extra vectors:

```cpp
pos
neg
ans
```

---

# Rearrange Array by Sign (O(1) Extra Space Follow-Up)

## Interview Follow-Up

**Question:**

Can we solve the Rearrange Array Elements by Sign problem using **O(1) extra space**?

### Answer

Yes, but only if:

* We do **not** need to preserve the relative order of elements.
* We are allowed to modify the array in-place.

If the problem requires maintaining the relative order of positives and negatives (as in LeetCode 2149), then the standard O(N) extra space solution is preferred.

---

# Idea

The approach consists of two steps:

### Step 1: Partition the Array

Move all negative numbers to the left side and all positive numbers to the right side.

This is similar to the partition process used in Quick Sort.

Example:

```text
Input:
[1, 2, -4, -5, 3, 4]

After Partition:
[-5, -4, 2, 1, 3, 4]
```

Notice that the original order is lost.

---

### Step 2: Alternate Positives and Negatives

Now:

```text
Negative Part -> [-5, -4]
Positive Part -> [2, 1, 3, 4]
```

Swap elements so that:

```text
[-5, 2, -4, 1, 3, 4]
```

Negatives appear at even positions and positives at odd positions.

---

# Algorithm

## Partition

Maintain index `i`.

Whenever a negative element is found:

1. Increment `i`
2. Swap `arr[i]` and `arr[j]`

After partition:

```cpp
0 ... i       -> negatives
i+1 ... n-1   -> positives
```

---

## Rearrange

Start:

```cpp
neg = 0
pos = i + 1
```

While:

```cpp
neg < pos
pos < n
arr[neg] < 0
```

Swap:

```cpp
swap(arr[neg], arr[pos]);
```

Move:

```cpp
neg += 2;
pos++;
```

---

# Code

```cpp
#include <bits/stdc++.h>
using namespace std;

void rearrange(int arr[], int n) {

    // Step 1: Partition negatives and positives

    int i = -1;

    for(int j = 0; j < n; j++) {

        if(arr[j] < 0) {
            i++;
            swap(arr[i], arr[j]);
        }
    }

    // First positive index

    int pos = i + 1;
    int neg = 0;

    // Step 2: Alternate positives and negatives

    while(pos < n &&
          neg < pos &&
          arr[neg] < 0) {

        swap(arr[neg], arr[pos]);

        pos++;
        neg += 2;
    }
}

int main() {

    int arr[] = {1, 2, -4, -5, 3, 4};

    int n = sizeof(arr) / sizeof(arr[0]);

    rearrange(arr, n);

    for(int x : arr)
        cout << x << " ";

    return 0;
}
```

---

# Dry Run

Input:

```text
[1, 2, -4, -5, 3, 4]
```

---

## Partition Phase

Move all negatives left.

```text
[-4, -5, 1, 2, 3, 4]
```

Negative ending index:

```text
i = 1
```

Positive starts at:

```text
pos = 2
```

---

## Rearrangement Phase

### Iteration 1

```text
neg = 0
pos = 2
```

Swap:

```text
[-4, -5, 1, 2, 3, 4]

↓

[1, -5, -4, 2, 3, 4]
```

Update:

```text
neg = 2
pos = 3
```

---

### Iteration 2

Swap:

```text
[1, -5, -4, 2, 3, 4]

↓

[1, -5, 2, -4, 3, 4]
```

Update:

```text
neg = 4
pos = 4
```

Stop.

---

Output:

```text
[1, -5, 2, -4, 3, 4]
```

---

# Complexity Analysis

### Time Complexity

Partition:

```text
O(N)
```

Rearrangement:

```text
O(N)
```

Overall:

```text
O(N)
```

---

### Space Complexity

```text
O(1)
```

Only a few variables are used.

No extra array is created.

---

# Important Observation

### O(N) Space Solution

✔ Preserves order

```text
[3,1,-2,-5,2,-4]
↓
[3,-2,1,-5,2,-4]
```

---

### O(1) Space Solution

✔ Constant extra space

❌ Does NOT preserve order

```text
[3,1,-2,-5,2,-4]
↓
[1,-5,2,-2,3,-4]
```

Order changes.

---

# Interview Answer

If asked:

"Can you optimize the space complexity?"

Answer:

> Yes. If preserving relative order is not required, we can first partition negatives and positives using the Quick Sort partition technique and then swap elements to place positives and negatives alternately. This achieves O(N) time and O(1) extra space, but the original order of elements is lost.