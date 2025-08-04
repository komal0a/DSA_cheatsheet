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
