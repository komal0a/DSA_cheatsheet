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
