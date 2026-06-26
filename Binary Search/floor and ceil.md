# 🔍 Floor and Ceil in a Sorted Array

---

# 📖 Problem Statement

Given a **sorted array** `nums` and an integer `target`, find:

- **Floor** → The **largest element** in the array that is **less than or equal to the target (<= target)**.
- **Ceil** → The **smallest element** in the array that is **greater than or equal to the target (>= target)**.

If either Floor or Ceil does not exist, return **-1** for that value.

---

# 💡 Important Observation

Floor and Ceil are almost opposite concepts.

```
Floor

Largest value <= target
```

```
Ceil

Smallest value >= target
```

---

# 🎯 Binary Search for Floor

Suppose

```
nums

2 4 6 8 10

target = 7
```

Initially

```
Floor = -1
```

---

### Rule

Whenever

```
nums[mid] <= target
```

This element **can become the Floor**.

Store it.

But maybe a larger valid element exists on the right.

So,

```
Move Right

low = mid+1
```

---

Whenever

```
nums[mid] > target
```

Current element cannot be the Floor.

Move Left.

```
high = mid-1
```

---

# ✍️ Floor Code

```cpp
int findFloor(vector<int>& nums,int target){

    int low = 0;
    int high = nums.size()-1;

    int floor = -1;

    while(low<=high){

        int mid = low + (high-low)/2;

        if(nums[mid] <= target){

            floor = nums[mid];
            low = mid + 1;

        }

        else{

            high = mid - 1;

        }
    }

    return floor;
}
```

# 🎯 Binary Search for Ceil

Initially

```
ceil=-1
```

Whenever

```
nums[mid]>=target
```

Current element is a possible Ceil.

Store it.

But maybe a smaller valid value exists on the left.

Move Left.

```
high=mid-1
```

---

Whenever

```
nums[mid]<target
```

Move Right.

```
low=mid+1
```

---

# ✍️ Ceil Code

```cpp
int findCeil(vector<int>& nums,int target){

    int low = 0;
    int high = nums.size()-1;

    int ceil = -1;

    while(low<=high){

        int mid = low + (high-low)/2;

        if(nums[mid] >= target){

            ceil = nums[mid];
            high = mid - 1;

        }

        else{

            low = mid + 1;

        }
    }

    return ceil;
}
```

---

# 🔥 Relation with Lower Bound

Lower Bound finds - First element >= target

definition of Ceil - Smallest element >= target

They are exactly the same! Therefore, Ceil can also be found using Lower Bound.

---

# 🔥 Relation with Upper Bound

Upper Bound finds - First element > target

Floor needs - Largest element <= target

So, Floor is simply, upperBound(target)-1

---

# 📊 Time Complexity

Finding Floor - O(log n)

Finding Ceil - O(log n)

Total - O(log n)

---

# 💾 Space Complexity

Only variables are used - O(1)

---

# ⚠️ Edge Cases

### Case 1

```
nums=[2,4,6]

target=1
```

```
Floor=-1

Ceil=2
```

---

### Case 2

```
nums=[2,4,6]

target=7
```

```
Floor=6

Ceil=-1
```

---

### Case 3

```
nums=[2,4,6]

target=4
```

```
Floor=4

Ceil=4
```

---

### Case 4

```
nums=[5]

target=5
```

```
Floor=5

Ceil=5
```

---

### Case 5

```
nums=[5]

target=2
```

```
Floor=-1

Ceil=5
```

---

# 🧠 Memory Trick

Or remember:

- **Floor** → "Go as far right as possible while staying ≤ target."
- **Ceil** → "Go as far left as possible while staying ≥ target."

---

# 🌟 Key Takeaways

- Floor = Largest value **≤ target**
- Ceil = Smallest value **≥ target**
- Floor moves **right** after finding a valid answer.
- Ceil moves **left** after finding a valid answer.
- Ceil is equivalent to the **Lower Bound value**.
- Floor can be obtained using **Upper Bound - 1**.
- Time Complexity = **O(log n)**
- Space Complexity = **O(1)**

---