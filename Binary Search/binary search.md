# 🔍 LeetCode 704 - Binary Search

---

# 📖 Problem Statement

Given a **sorted (ascending order)** integer array `nums` and an integer `target`, return the **index** of `target` if it exists.

If the target is **not present**, return **-1**.

---

# 🤔 Intuition

Suppose you're searching for a word in a dictionary.

Do you start from page 1?

No.

You directly open somewhere in the middle.

- If your word comes **after** that page, ignore the left half.
- If your word comes **before**, ignore the right half.
- Repeat until you find it.

Binary Search follows exactly this idea.

Instead of checking every element, it repeatedly cuts the search space into half.

---

# 🚀 Why Binary Search is Faster?

Suppose an array has

```
1,000,000 elements
```

### Linear Search

Worst case:

```
Check all 1,000,000 elements.
```

Time Complexity:

```
O(n)
```

---

### Binary Search

Every comparison removes **half** of the remaining elements.

```
1000000

↓

500000

↓

250000

↓

125000

↓

62500

↓

31250

↓

...

↓

1
```

Only about

```
log₂(1000000) ≈ 20
```

comparisons!

That's why Binary Search is extremely efficient.

---

# 🎯 Key Observation

At every iteration,

we compare the middle element.

There are only three possibilities.

### Case 1

```
nums[mid] == target
```

We found the answer.

Return the index.

---

### Case 2

```
nums[mid] < target
```

Target is larger.

Since the array is sorted,

everything on the left of mid is even smaller.

So we can safely ignore the entire left half.

Move

```
low = mid + 1
```

---

### Case 3

```
nums[mid] > target
```

Target is smaller.

Everything after mid is larger.

Ignore the right half.

Move

```
high = mid - 1
```

---

# ✅ Optimal Solution (Binary Search)

## Idea

Maintain two pointers

```
low
high
```

Compute the middle.

Check which side can be discarded.

Repeat until

```
low > high
```

---

# 🧠 Why do we calculate mid like this?

Instead of

```cpp
mid = (low + high) / 2;
```

we write

```cpp
mid = low + (high - low) / 2;
```

### Why?

Suppose

```
low = 2,000,000,000

high = 2,100,000,000
```

Then

```
low + high
```

can exceed the integer limit.

This is called **Integer Overflow**.

Instead,

```
high - low
```

is always much smaller.

Hence

```cpp
low + (high-low)/2
```

is the safer way.

---

# ✍️ Code

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {

        int n = nums.size();

        int low = 0;
        int high = n - 1;

        while(low <= high){

            int mid = low + (high - low)/2;

            if(nums[mid] == target){
                return mid;
            }

            else if(nums[mid] < target){
                low = mid + 1;
            }

            else{
                high = mid - 1;
            }
        }

        return -1;
    }
};
```

---

# 🖼️ Visual Representation

```
Initial

L           H

1 3 5 7 9 11 13

      M
```

Suppose target is

```
11
```

Since

```
5 < 11
```

Ignore everything on the left.

```
        L     H

7 9 11 13

     M
```

Again

```
11 found
```

---

# ✅ Why Does This Work?

The array is sorted.

If

```
nums[mid] < target
```

everything before mid is also smaller.

There is absolutely no chance of finding the target there.

Similarly,

if

```
nums[mid] > target
```

everything after mid is larger.

So that half can also be discarded.

Thus every iteration safely removes **half** the search space.

---

# 📈 Time Complexity Analysis

Let

```
n = number of elements
```

Each iteration removes half.

```
n

↓

n/2

↓

n/4

↓

n/8

↓

...

↓

1
```

The number of halvings is

```
log₂(n)
```

Therefore,

### Time Complexity

```
O(log n)
```

---

# 💾 Space Complexity

We only use

```
low

high

mid
```

No extra array.

### Space Complexity

```
O(1)
```

---

# ⚠️ Edge Cases

### Case 1

Single element

```
[5]

target=5
```

Answer

```
0
```

---

### Case 2

Single element

```
[5]

target=2
```

Answer

```
-1
```

---

### Case 3

Target is first element

```
[1,2,3,4]

target=1
```

---

### Case 4

Target is last element

```
[1,2,3,4]

target=4
```

---

### Case 5

Empty array

```
[]
```

Loop never executes.

Returns

```
-1
```

---

# 🌟 Key Takeaways

- Binary Search works **only on sorted data.**
- Eliminate **half** of the search space in every iteration.
- Use **low + (high - low) / 2** to avoid integer overflow.
- Always update:
  - `low = mid + 1`
  - `high = mid - 1`
- Standard Binary Search runs in:
  - **Time:** `O(log n)`
  - **Space:** `O(1)`
- This template is the foundation for many problems like:
  - Lower Bound
  - Upper Bound
  - Search Insert Position
  - Floor & Ceil
  - First/Last Occurrence
  - Peak Element
  - Search in Rotated Sorted Array
  - Binary Search on Answer

---