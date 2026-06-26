# 🔍 LeetCode 35 - Search Insert Position LC - 35

---

# 📖 Problem Statement

Given a **sorted array of distinct integers** `nums` and a target value `target`, return the **index** if the target is found.

If the target is **not found**, return the index where it **would be inserted** while maintaining the sorted order.

You **must** write an algorithm with **O(log n)** runtime complexity.


---

# 💡 Intuition

Suppose

```
nums = [1,3,5,6]

target = 4
```

Where should 4 be inserted?

```
1 3 | 5 6
    ↑
```

Answer:

```
Index = 2
```

Notice something interesting.

We are looking for the **first element that is greater than or equal to the target**.

```
5 >= 4
```

That is exactly the definition of **Lower Bound**.

---

# 🎯 Key Observation

Search Insert Position is nothing but **Lower Bound**.

> Lower Bound = First index whose value is **greater than or equal to the target (>= target).**

---

# 🔥 Important Observation

This problem **never asks us to actually insert** the target.

It only asks for

```
the position
```

where insertion should happen.

---

# ✅ Optimal Solution (Binary Search)

Instead of checking every element,

use Binary Search.

---

# 🤔 Why do we return `low`?

This is the most important part of this problem.

Suppose

```
nums=[1,3,5,6]

target=2
```

Initially

```
low=0

high=3
```

---

Iteration 1

```
mid=1

nums[mid]=3
```

```
3>2
```

Move left.

```
high=0
```

---

Iteration 2

```
mid=0

nums[mid]=1
```

```
1<2
```

Move right.

```
low=1
```

Now

```
low=1

high=0
```

Loop stops.

Notice

```
low
```

points exactly where

```
2
```

should be inserted.

---

This is **not a coincidence**.

Whenever Binary Search ends,

```
low
```

becomes the **Lower Bound**.

Therefore,

```
return low;
```

---

# 📌 Why not return `high`?

When the loop finishes,

```
high
```

points to the **largest element smaller than target**.

```
low
```

points to the **smallest element greater than or equal to target**.

The question asks for the insertion position.

That is exactly

```
low
```

---

# ✍️ Code

```cpp
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {

        int low = 0;
        int high = nums.size()-1;

        while(low<=high){

            int mid = low + (high-low)/2;

            if(nums[mid]==target)
                return mid;

            else if(nums[mid]<target)
                low = mid+1;

            else
                high = mid-1;
        }

        return low;
    }
};
```

---

# 📝 Dry Run 1 (Target Exists)

```
nums=[1,3,5,6]

target=5
```

Initially

```
low=0

high=3
```

---

Iteration 1

```
mid=1

nums[mid]=3
```

```
3<5
```

Move right.

```
low=2
```

---

Iteration 2

```
mid=2

nums[mid]=5
```

Found.

Return

```
2
```

---

# 📝 Dry Run 2 (Target Doesn't Exist)

```
nums=[1,3,5,6]

target=2
```

Initially

```
low=0

high=3
```

---

Iteration 1

```
mid=1

nums[mid]=3
```

Move left.

```
high=0
```

---

Iteration 2

```
mid=0

nums[mid]=1
```

Move right.

```
low=1
```

Loop ends.

```
low=1

high=0
```

Return

```
1
```

---

# 📝 Dry Run 3 (Insert at End)

```
nums=[1,3,5,6]

target=8
```

Iteration 1

```
mid=1
```

Move right.

```
low=2
```

---

Iteration 2

```
mid=2
```

Move right.

```
low=3
```

---

Iteration 3

```
mid=3
```

Move right.

```
low=4
```

Loop ends.

Return

```
4
```

---

# 📝 Dry Run 4 (Insert at Beginning)

```
nums=[1,3,5,6]

target=0
```

Eventually,

```
low=0

high=-1
```

Return

```
0
```

---

### Time Complexity

```
O(log n)
```

---

### Space Complexity

```
O(1)
```

---

# 🌟 Key Takeaways

- Search Insert Position is an application of **Lower Bound**.
- Return the existing index if the target is found.
- If not found, return the position where it should be inserted.
- After Binary Search ends, `low` always represents the correct insertion position.
- Never return `high`.
- Time Complexity: **O(log n)**
- Space Complexity: **O(1)**

---