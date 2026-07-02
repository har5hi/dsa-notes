# Square Root of a Number (Floor Value)

## Problem Statement

Given a positive integer `n`, return:

```cpp
⌊√n⌋
```

If `n` is a perfect square:

```cpp
return √n
```

Otherwise:

```cpp
return floor(√n)
```

---

# Brute Force Approach

## Idea

Check every number from:

```cpp
1 to n
```

Find the largest number whose square is less than or equal to `n`.

---

## Code

```cpp
int floorSqrt(int n) {

    int ans = 1;

    for(int i = 1; i <= n; i++) {

        long long square = 1LL * i * i;

        if(square <= n)
            ans = i;
        else
            break;
    }

    return ans;
}
```

---

## Complexity Analysis

| Complexity | Value |
|------------|--------|
| Time | O(n) |
| Space | O(1) |

---

# Better Observation

For any number:

```cpp
mid
```

If:

```cpp
mid * mid <= n
```

Then `mid` can be a possible answer.

Try finding a larger value.

---

If:

```cpp
mid * mid > n
```

Then `mid` is too large.

Search on the left side.

---

# Why Binary Search Works

Consider:

```cpp
n = 36
```

Squares:

```text
1² = 1
2² = 4
3² = 9
4² = 16
5² = 25
6² = 36
7² = 49
```

Notice:

```text
1  4  9  16  25  36 | 49 ...
```

Once we cross `n`:

```cpp
mid² > n
```

all larger numbers will also have:

```cpp
mid² > n
```

This monotonic behavior allows Binary Search.

---

# Search Space

Possible square root lies between:

```cpp
1 and n
```

---

# Important Trick

Always use:

```cpp
long long
```

while calculating:

```cpp
mid * mid
```

Because:

```cpp
46341 * 46341
```

already exceeds integer range.

Use:

```cpp
long long val = 1LL * mid * mid;
```

to avoid overflow.

---

# Optimal Code

```cpp
int floorSqrt(int n) {

    int low = 1;
    int high = n;

    while(low <= high) {

        long long mid = low + (high - low) / 2;

        long long val = mid * mid;

        if(val <= n) {
            low = mid + 1;
        }
        else {
            high = mid - 1;
        }
    }

    return high;
}
```

---

# Why Return `high`?

At the end:

```cpp
low > high
```

The last valid value whose square was:

```cpp
<= n
```

is stored in:

```cpp
high
```

Therefore:

```cpp
return high;
```

gives:

```cpp
⌊√n⌋
```

---

# Complexity Analysis

| Complexity | Value |
|------------|--------|
| Time | O(log n) |
| Space | O(1) |

---

# Why Binary Search on Answer?

We are not searching inside an array. We are searching for an answer in a range:

```cpp
1 → n
```

and checking:

```cpp
mid² <= n ?
```

This is a classic:

```text
Binary Search on Answer
```

pattern.

---