# 🔍 Lower Bound & Upper Bound (Binary Search)
---

# 📖 Introduction

After learning the basic Binary Search, the next important concept is **Lower Bound** and **Upper Bound**.

Many Binary Search interview questions are **direct applications** of these concepts.

---

# 📌 Definition of Lower Bound

The **Lower Bound** of a target is

> **the first index whose value is greater than or equal to the target (>= target).**

Mathematically,

```
nums[index] >= target
```

and it must be the **first** such index.

---

# ✍️ Lower Bound Code

```cpp
int lowerBound(vector<int>& nums, int target){

    int low = 0;
    int high = nums.size()-1;

    int ans = nums.size();

    while(low <= high){

        int mid = low + (high-low)/2;

        if(nums[mid] >= target){

            ans = mid;
            high = mid - 1;

        }
        else{

            low = mid + 1;

        }
    }

    return ans;
}
```

---

# 📈 Time Complexity

Every iteration removes half.

```
O(log n)
```

---

# 💾 Space Complexity

Only variables are used.

```
O(1)
```

---

# 🤔 What is Upper Bound?

Upper Bound is very similar.

# 📌 Definition of Upper Bound

Upper Bound is

> **the first index whose value is strictly greater than the target (> target).**

---

# ✍️ Upper Bound Code

```cpp
int upperBound(vector<int>& nums, int target){

    int low = 0;
    int high = nums.size()-1;

    int ans = nums.size();

    while(low <= high){

        int mid = low + (high-low)/2;

        if(nums[mid] > target){

            ans = mid;
            high = mid - 1;

        }

        else{

            low = mid + 1;

        }
    }

    return ans;
}
```

---

# 🔥 Lower Bound vs Upper Bound

| Lower Bound | Upper Bound |
|-------------|-------------|
| First element **>= target** | First element **> target** |
| `nums[mid] >= target` | `nums[mid] > target` |
| Used in Search Insert Position | Used in Count Occurrences |
| Can point to target itself | Always points after the last occurrence of target |

---

# 📚 Relationship with Duplicates

Suppose

```
nums=[2,2,2,2,2]
```

Target

```
2
```

Lower Bound

```
0
```

Upper Bound

```
5
```

Number of occurrences

```
upperBound - lowerBound

5-0=5
```

This trick is commonly used in interviews.

---

# 🛠️ STL Functions

C++ already provides these functions.

Lower Bound

```cpp
lower_bound(nums.begin(), nums.end(), target)
```

Upper Bound

```cpp
upper_bound(nums.begin(), nums.end(), target)
```

Both return an **iterator**, not the index.

To convert to an index:

```cpp
int index = lower_bound(nums.begin(), nums.end(), target) - nums.begin();
```

```cpp
int index = upper_bound(nums.begin(), nums.end(), target) - nums.begin();
```

---

# 🎯 Common Interview Applications

Lower Bound is used in:

- Search Insert Position (LC 35)
- First Occurrence
- Floor & Ceil
- Minimum Element ≥ X
- Binary Search on Answers

Upper Bound is used in:

- Last Occurrence
- Count Occurrences
- First Element > X
- Range Queries

---

# 🧠 Memory Trick

- **Lower** = includes the target (`>=`)
- **Upper** = goes past the target (`>`)

---

# 🌟 Key Takeaways

- Lower Bound = **first index where value ≥ target**
- Upper Bound = **first index where value > target**
- Both are implemented using Binary Search.
- Never stop after finding the target; continue searching for the first valid position.
- Initialize `ans = n` because the answer may be the insertion position at the end.
- Both algorithms run in:
  - **Time:** `O(log n)`
  - **Space:** `O(1)`
- Understanding these two concepts makes problems like **Search Insert Position, Floor & Ceil, First/Last Occurrence, and Count Occurrences** much easier.

---