# Book Allocation Problem

## Problem Statement

Given:

```cpp
arr[]
```

where:

```cpp
arr[i]
```

represents the number of pages in the `i-th` book.

And:

```cpp
m
```

students.

Allocate books such that:

- Each student gets at least one book.
- Books are allocated contiguously.
- A book cannot be split.
- Every book must be allocated.

Return the **minimum possible maximum pages assigned to any student**.

If allocation is impossible:

```cpp
return -1;
```

---

## Example

```cpp
Input:

arr = [25,46,28,49,24]
m = 4
```

Output:

```cpp
71
```

---

### Allocation

```text
Student 1 → 25 + 46 = 71

Student 2 → 28

Student 3 → 49

Student 4 → 24
```

Maximum pages:

```cpp
71
```

Minimum possible answer.

---

# Brute Force Approach (Linear Search on Answer)

## Observation

The answer cannot be smaller than:

```cpp
max(arr)
```

because every book must be assigned.

---

The answer cannot be larger than:

```cpp
sum(arr)
```

because one student can take all books.

---

Search every possible answer:

```cpp
max(arr) → sum(arr)
```

and find the first valid one.

---

# Helper Function

For a given page limit:

```cpp
pages
```

calculate how many students are needed.

---

## Greedy Allocation

Keep assigning books.

If adding the next book exceeds limit:

```cpp
students++
```

assign the book to a new student.

---

# Brute Force Code

```cpp
class Solution {
public:

    int countStudents(vector<int>& arr, int pages) {

        int students = 1;
        long long pagesStudent = 0;

        for(int book : arr) {

            if(pagesStudent + book <= pages) {
                pagesStudent += book;
            }
            else {
                students++;
                pagesStudent = book;
            }
        }
        return students;
    }

    int findPages(vector<int>& arr,
                  int m) {

        int n = arr.size();

        if(m > n) return -1;

        int low = *max_element(arr.begin(), arr.end());
        int high = accumulate(arr.begin(), arr.end(), 0);

        for(int pages = low; pages <= high; pages++) {
            if(countStudents(arr, pages) <= m) {
                return pages;
            }
        }
        return -1;
    }
};
```

---

# Complexity Analysis

### Linear Search

```cpp
O(sum(arr) - max(arr))
```

### Allocation Check

```cpp
O(n)
```

### Total

```cpp
O(n × (sum-max))
```
Too slow.

---

# Optimal Approach (Binary Search on Answer)

## Key Observation

Suppose:

```cpp
pages = 100
```

works.

Then:

```cpp
101 102 103
```

will also work.

---

Suppose:

```cpp
pages = 70
```

does not work.

Then:

```cpp
69 68 67
```

will never work.

---

This creates a monotonic pattern:

```text
Pages Limit

50 60 70 80 90 100
✗  ✗  ✗  ✓  ✓  ✓
```

We need:

```text
First Valid Answer
```

Hence Binary Search.

---

# Binary Search Logic

### If

```cpp
studentsNeeded <= m
```

Allocation possible. Try smaller answer.

```cpp
high = mid - 1
```

---

### If

```cpp
studentsNeeded > m
```

Need larger page limit.

```cpp
low = mid + 1
```

---

# Optimal Code

```cpp
class Solution {
public:

    int countStudents(vector<int>& arr, int pages) {

        int students = 1;
        long long pagesStudent = 0;

        for(int book : arr) {

            if(pagesStudent + book <= pages) {
                pagesStudent += book;
            }
            else {
                students++;
                pagesStudent = book;
            }
        }
        return students;
    }

    int findPages(vector<int>& arr, int m) {

        int n = arr.size();

        if(m > n) return -1;

        int low = *max_element(arr.begin(), arr.end());

        int high = accumulate(arr.begin(), arr.end(), 0);

        while(low <= high) {

            int mid = low + (high - low) / 2;

            int studentsNeeded = countStudents(arr, mid);

            if(studentsNeeded <= m) {
                high = mid - 1;
            }
            else {
                low = mid + 1;
            }
        }
        return low;
    }
};
```

---

# Complexity Analysis

Let:

```cpp
n = number of books
```

Let:

```cpp
S = sum(arr)
```

---

### Binary Search

```cpp
O(log S)
```

### Allocation Check

```cpp
O(n)
```

### Total

| Complexity | Value |
|------------|--------|
| Time | O(n × log(sum(arr))) |
| Space | O(1) |

---

# Why Greedy Allocation Works?

For a fixed page limit:

```cpp
mid
```

we always try to fill the current student as much as possible.
This minimizes the number of students used.
Hence the greedy strategy gives the correct count.

---