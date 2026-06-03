# Check if an Array is Sorted

## Problem Statement

Given an array of integers, determine whether the array is sorted in non-decreasing (ascending) order.

---

## Approach

1. Traverse the array from index `1` to `n-1`.
2. Compare each element with its previous element.
3. If `arr[i] < arr[i-1]`, the array is not sorted.
4. Otherwise continue checking.
5. If no violation is found, the array is sorted.

---

## C++ Solution

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {

    int arr[] = {1, 2, 3, 4, 5};

    int n = sizeof(arr) / sizeof(arr[0]);

    bool sorted = true;

    for(int i = 1; i < n; i++) {

        if(arr[i] < arr[i - 1]) {
            sorted = false;
            break;
        }
    }

    if(sorted)
        cout << "ARRAY IS SORTED";
    else
        cout << "ARRAY IS NOT SORTED";

    return 0;
}
```

---
## Mistakes I Made While Solving

### 1. Accessing `arr[i-1]` when `i = 0`

Incorrect:

```cpp
for(int i = 0; i < n; i++) {
    if(arr[i] >= arr[i-1])
}
```

Issue:

When `i = 0`, the expression becomes:

```cpp
arr[-1]
```

which is invalid.

Correct:

```cpp
for(int i = 1; i < n; i++)
```

---

### 2. Returning Inside the Loop

Incorrect:

```cpp
if(arr[i] >= arr[i-1]) {
    return true;
}
```

Issue:

The program exits after checking only the first comparison.

Correct:

```cpp
bool sorted = true;

for(int i = 1; i < n; i++) {
    if(arr[i] < arr[i-1]) {
        sorted = false;
        break;
    }
}
```

---

### 3. Writing `cout` After `return`

Incorrect:

```cpp
return true;
cout << "ARRAY IS SORTED";
```

Issue:

Any statement after `return` is never executed.

Correct:

```cpp
cout << "ARRAY IS SORTED";
return 0;
```

---

### 4. Using `return true` and `return false` in `main()`

Incorrect:

```cpp
return true;
return false;
```

Issue: Although C++ converts them to integers, `main()` should return an integer status code.

Correct:

```cpp
return 0;
```

---

## Time Complexity

* Time Complexity: O(n)
* Space Complexity: O(1)

The array is traversed only once, making the solution efficient.

---
