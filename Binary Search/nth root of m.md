# Find Nth Root of M

## Problem Statement

Given two positive integers:

```cpp
N and M
```

Return the **Nth root of M**.

If the Nth root is an integer, return it.

Otherwise return:

```cpp
-1
```

---

## Code

```cpp
int NthRoot(int N, int M) {

    for(int i = 1; i <= M; i++) {

        long long val = 1;

        for(int j = 1; j <= N; j++) {
            val *= i;
        }

        if(val == M)
            return i;

        if(val > M)
            break;
    }

    return -1;
}
```

---

## Complexity Analysis

| Complexity | Value |
|------------|--------|
| Time | O(M × N) |
| Space | O(1) |

Very inefficient for large values.

---

# Optimal Approach (Binary Search)

## Observation

Suppose:

```cpp
N = 3
M = 27
```

Possible answers:

```text
1 2 3 4 5 ...
```

Check:

```cpp
1^3 = 1
2^3 = 8
3^3 = 27
4^3 = 64
```

As x increases:

```cpp
x^N also increases
```

This is a monotonic property. Therefore Binary Search can be applied.

---

# Search Space

Possible root lies between:

```cpp
1 and M
```

---

# Important Helper Function

For a given number:

```cpp
mid
```

we calculate:

```cpp
mid^N
```

and return:

### Return 1

If:

```cpp
mid^N == M
```

Meaning root found.

---

### Return 0

If:

```cpp
mid^N < M
```

Need larger number.

---

### Return 2

If:

```cpp
mid^N > M
```

Need smaller number.

---

# Why Use Early Termination?

Suppose:

```cpp
mid = 100
N = 10
```

Computing:

```cpp
100^10
```

can overflow.

So while multiplying:

```cpp
ans *= mid
```

if:

```cpp
ans > M
```

immediately return:

```cpp
2
```

No need to continue! This prevents overflow and saves time.

---

# Optimal Code

```cpp
class Solution {
public:

    int func(int mid, int n, int m) {

        long long ans = 1;

        for(int i = 1; i <= n; i++) {

            ans *= mid;

            if(ans > m)
                return 2;
        }

        if(ans == m)
            return 1;

        return 0;
    }

    int NthRoot(int n, int m) {

        int low = 1;
        int high = m;

        while(low <= high) {

            int mid = low + (high - low) / 2;

            int midN = func(mid, n, m);

            if(midN == 1)
                return mid;

            else if(midN == 0)
                low = mid + 1;

            else
                high = mid - 1;
        }

        return -1;
    }
};
```

---

# Why Binary Search Works

For every number x:

```cpp
x^N
```

is strictly increasing.

Example:

```text
x      x³
-------------
1  ->   1
2  ->   8
3  ->  27
4  ->  64
5  -> 125
```

Since values increase monotonically:

```cpp
If mid^N < M
```

answer lies right.

```cpp
If mid^N > M
```

answer lies left.

Hence Binary Search is valid.

---

# Complexity Analysis

### Helper Function

Computes:

```cpp
mid^N
```

in:

```cpp
O(N)
```

---

### Binary Search

Runs:

```cpp
O(log M)
```

times.

---

### Total Complexity

| Complexity | Value |
|------------|--------|
| Time | O(N × log M) |
| Space | O(1) |

---

### Helper Function Returns

```cpp
1 → mid^N == M
0 → mid^N < M
2 → mid^N > M
```

---

### Binary Search Decisions

```cpp
0 → search right
2 → search left
1 → answer found
```

---

# Pattern Recognition
This problem belongs to:
```text
Binary Search on Answer
```
Common clues:
✓ Find minimum value
✓ Find maximum value
✓ Find root
✓ Search in a monotonic function
✓ Answer lies in a range