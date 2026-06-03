# Second Smallest and Second Largest Element in an Array

## Problem Statement

Given an array, find the second smallest and second largest element in the array.

Print `-1` if either the second smallest or second largest element does not exist.

## Approach

1. Find the smallest and largest elements.
2. Traverse the array again.
3. Update the second smallest whenever an element is:

   * Greater than the smallest.
   * Smaller than the current second smallest.
4. Update the second largest whenever an element is:

   * Smaller than the largest.
   * Greater than the current second largest.
5. If a valid second smallest or second largest is not found, print `-1`.

---

## Mistakes I Made While Solving

### 1. Forgetting the Edge Case `n < 2`

* Causes out-of-bounds access when the array contains fewer than two elements.

Correct:

```cpp
if(n < 2)
{
    cout << -1;
    return 0;
}
```

---

### 2. Not Checking Whether a Valid Answer Exists

Example:

```cpp
{5, 5, 5, 5}
```

There is no second smallest or second largest.

Correct:

```cpp
if(secondSmallest == INT_MAX ||
   secondLargest == INT_MIN)
{
    cout << -1;
}
```

---

## Code :

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int arr[] = {1, 2, 4, 7, 7, 5};

    int n = sizeof(arr) / sizeof(arr[0]);

    if (n < 2) {
        cout << -1;
        return 0;
    }

    int smallest = INT_MAX;
    int secondSmallest = INT_MAX;

    int largest = INT_MIN;
    int secondLargest = INT_MIN;

    for (int i = 0; i < n; i++) {

        if (arr[i] < smallest) {
            secondSmallest = smallest;
            smallest = arr[i];
        }
        else if (arr[i] > smallest && arr[i] < secondSmallest) {
            secondSmallest = arr[i];
        }

        if (arr[i] > largest) {
            secondLargest = largest;
            largest = arr[i];
        }
        else if (arr[i] < largest && arr[i] > secondLargest) {
            secondLargest = arr[i];
        }
    }

    if (secondSmallest == INT_MAX || secondLargest == INT_MIN) {
        cout << -1;
    }
    else {
        cout << "Second Smallest = " << secondSmallest << endl;
        cout << "Second Largest = " << secondLargest << endl;
    }

    return 0;
}
```

---
## Time Complexity

* Time Complexity: O(n)
* Space Complexity: O(1)

The array is traversed only once, making this an optimal solution.
