## 🔁 Set Matrix Zeroes — Leetcode 73

**Approach:**  
Use the **first row and first column** as markers to flag which rows/cols should be zeroed.  
Track the first column separately using a variable `col0`.

### 🧠 Key Steps:
1. **First pass (Top-Down):**
   - For each `matrix[i][j]`:
     - If `matrix[i][j] == 0`:
       - Mark `matrix[i][0] = 0` (row flag)
       - Mark `matrix[0][j] = 0` (col flag)
       - If `j == 0`, set `col0 = 0`

2. **Second pass (Top-Down again):**
   - For each `matrix[i][j]` (excluding first row and col):
     - If `matrix[i][0] == 0` or `matrix[0][j] == 0`, set `matrix[i][j] = 0`
   - Then handle first row and first column at the end using `col0` and `matrix[0][j]`

### ⏱️ Complexity:
- **Time:** O(n × m)
- **Space:** O(1)


# Pascal’s Triangle – Striver’s SDE Sheet

Striver covers Pascal's Triangle in three main variations. Below is a cheat sheet summarizing each one with explanation and time-space complexities.

---

## ✅ Variation 1: Print the Entire Pascal’s Triangle up to `n` Rows

- **Task**: Return a 2D vector containing the first `n` rows of Pascal’s Triangle.
- **Logic**:  
  - Every row starts and ends with `1`.
  - Middle elements follow the rule:  
    `row[i][j] = row[i-1][j-1] + row[i-1][j]`
- **Example (n = 5)**:
- **Time Complexity**: O(n²)  
- **Space Complexity**: O(n²)

---

## ✅ Variation 2: Print Only the `n`th Row (1-based)

- **Task**: Return the `n`th row of Pascal’s Triangle as a vector.
- **Logic**:  
- Use the combination formula iteratively:  
  `nCr = nC(r-1) * (n - r + 1) / r`
- **Example (n = 4)**:
- **Time Complexity**: O(n)  
- **Space Complexity**: O(n)

---

## ✅ Variation 3: Get the Value at a Specific Cell (r, c)

- **Task**: Return the value at row `r` and column `c` (0-indexed).
- **Logic**:  
- Use direct formula:  
  `nCr = n! / (r! * (n - r)!)`  
  Or use iterative method to avoid large factorials.
- **Example**:  
Value at (4, 2) = `6` (Because it's 4C2)
- **Time Complexity**: O(r)  
- **Space Complexity**: O(1)

---
## Kadane’s Algorithm – Maximum Subarray Sum

Kadane's Algorithm is used to find the maximum sum of a contiguous subarray in a given integer array.

### 🔸 Key Idea
Keep a running sum (`curSum`) and reset it to 0 whenever it drops below 0. Track the maximum value seen so far (`maxSum`).

### 🧠 Intuition
If adding the current element drops the sum below 0, that subarray can't contribute to a maximum sum ahead—so reset.

### 📘 Dry Run Example
Given `arr = [-2, 1, -3, 4, -1, 2, 1, -5, 4]`:

1. Start with `curSum = 0`, `maxSum = -∞`
2. Update `curSum` at each index:
   - If `curSum < 0`, reset to 0
   - Update `maxSum = max(maxSum, curSum)`

Final `maxSum = 6` for subarray `[4, -1, 2, 1]`

### ✅ Time & Space
- Time: `O(n)`
- Space: `O(1)`

- ## 🎨 Sort Colors (Dutch National Flag Problem)

### 🧩 Problem
Given an array `nums` with `n` objects colored red (`0`), white (`1`), or blue (`2`), sort them **in-place** so that objects of the same color are adjacent and in the order red → white → blue.

### 🔧 Constraints
- Do not use library sort functions.
- Must solve in one pass using **constant space**.

---

### ✅ Optimal Approach: Dutch National Flag Algorithm

#### 💡 Idea
Use three pointers:
- `low` — boundary for 0s
- `mid` — current element
- `high` — boundary for 2s

#### 🔁 Logic
- If `nums[mid] == 0`: swap with `low`, increment both
- If `nums[mid] == 1`: move `mid` forward
- If `nums[mid] == 2`: swap with `high`, decrement `high`

#### ⏱️ Time & Space
- Time: `O(n)`
- Space: `O(1)` — in-place

---

### 🧠 Intuition
You're essentially grouping elements by pushing:
- All `0`s to the front
- All `2`s to the end
- Leaving `1`s in the middle
Just like arranging flags in the Dutch flag!

---

### 🏷️ Tags
`Array`, `Two Pointers`, `Sorting`, `In-place Algorithm`

### 🧾 Description

You are given an array of intervals where `intervals[i] = [start_i, end_i]`.  
Merge all overlapping intervals and return an array of the non-overlapping intervals that cover all the intervals in the input.

---

### 📌 Approach

1. **Sort** the intervals based on their starting point.
2. Initialize a temp interval with the first one.
3. Traverse the list:
   - If the current interval overlaps with temp, merge them.
   - If not, push temp to answer and move on.
4. Push the last interval to the result.

---

### 💡 Time & Space Complexity

- **Time:** `O(n log n)` – due to sorting
- **Space:** `O(n)` – for the result vector

---

### 👨‍💻 Code

```cpp
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        vector<vector<int>> ans;
        int n = intervals.size();

        if (n == 0) return ans;

        sort(intervals.begin(), intervals.end());

        vector<int> temp = intervals[0];

        for (int i = 1; i < n; i++) {
            if (temp[1] >= intervals[i][0]) {
                temp[1] = max(temp[1], intervals[i][1]);
            } else {
                ans.push_back(temp);
                temp = intervals[i];
            }
        }

        ans.push_back(temp);
        return ans;
    }
};

## 287. Find the Duplicate Number

**Difficulty**: Medium  
**Link**: [LeetCode 287](https://leetcode.com/problems/find-the-duplicate-number/)

### Problem
Given an array `nums` containing `n + 1` integers where each integer is in the range `[1, n]` inclusive.  
There is only one repeated number in `nums`, return this repeated number.

You must solve the problem **without modifying** the array and using only **constant extra space**.

---

### Approach

We use **Floyd's Tortoise and Hare (Cycle Detection)** method to find the duplicate number.

- Treat the array as a linked list, where `nums[i]` is the next node.
- The cycle entry point is the duplicate number.

---

### Code

```cpp
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        int slow = nums[0];
        int fast = nums[0];

        // Phase 1: Detect the cycle
        do {
            slow = nums[slow];
            fast = nums[nums[fast]];
        } while (slow != fast);

        // Phase 2: Find the entrance to the cycle
        slow = nums[0];
        while (slow != fast) {
            slow = nums[slow];
            fast = nums[fast];
        }

        return slow;
    }
};


