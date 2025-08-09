class Solution {
public:
    void solve(vector<int>& candidates,int n, int ind,int target, vector<int>& temp, vector<vector<int>>&ans){
       if(target==0){
           ans.push_back(temp);
           return;
        }

        if (ind == n || target < 0) return;

        temp.push_back(candidates[ind]);
        solve(candidates,n,ind,target-candidates[ind],temp,ans);
        temp.pop_back();
        solve(candidates,n,ind+1,target,temp,ans);


    }
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<int>temp;
        vector<vector<int>>ans;
        int n= candidates.size();
        solve(candidates, n, 0,target,temp,ans);
        return ans;
    }
};

# 🌟 90. Subsets II

## ✅ Problem Statement

Given an integer array `nums` that may contain duplicates, return **all possible subsets** (the power set) that **do not contain duplicate subsets**. The solution set must **not contain duplicate subsets**, and subsets can be in any order.

---

## 💡 Intuition

This is a variation of the **subsets/backtracking** problem with one twist: **input array contains duplicates**.

To handle duplicates:
- First **sort the array**.
- While skipping elements, **jump over all duplicates** in the recursive call.
  - This ensures each duplicate number is either:
    - included in the subset, OR
    - completely skipped

---

## 🧠 Approach

1. Sort `nums` to bring duplicates together.
2. Use **backtracking** to explore all subsets.
3. On **excluding** an element, **skip all duplicates** to avoid generating same subset again.

---

## 🔁 Dry Run

For `nums = [1, 2, 2]`:

Sorted: `[1, 2, 2]`

Subsets generated:


# 💡 Dry Run and Explanation of `Combination Sum II` Problem

## ✅ Problem Statement

Given a list of `candidates` (may include duplicates) and a `target`, return all unique combinations where the candidate numbers sum to `target`. Each number in `candidates` may only be used **once** in the combination.

---

## 🔧 Code Used

```cpp
class Solution {
public:
    void solve(vector<int>& candidates, int n, int ind, int target, vector<int>& temp, vector<vector<int>>& ans) {
        if (target == 0) {
            ans.push_back(temp);
            return;
        }

        for (int i = ind; i < n; i++) {
            if (i > ind && candidates[i] == candidates[i - 1]) continue;
            if (candidates[i] > target) break;

            temp.push_back(candidates[i]);
            solve(candidates, n, i + 1, target - candidates[i], temp, ans);
            temp.pop_back();
        }
    }

    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        sort(candidates.begin(), candidates.end());
        vector<int> temp;
        vector<vector<int>> ans;
        int n = candidates.size();
        solve(candidates, n, 0, target, temp, ans);
        return ans;
    }
};

class Solution {
public:
    int n;
    bool ispalindrome(string& s , int start, int end){
        while(start<end){
            if(s[start++]!=s[end--])return false;
        }
        return true;
    }

    void solve(vector<string>&temp,vector<vector<string>>&ans,string s, int i){
        if(i==n){
            ans.push_back(temp);
            return;
        }

        for(int j=i;j<n;j++){
            if(ispalindrome(s,i,j)){
            temp.push_back(s.substr(i, j - i + 1));
            solve(temp,ans,s,j+1);
            temp.pop_back();
            }
        }
    }
    vector<vector<string>> partition(string s) {
        vector<string>temp;
        vector<vector<string>>ans;
        n= s.length();

        solve(temp,ans,s,0);
        return ans;
    }
};

class Solution {
public:

    void solve(vector<int>& nums, vector<int>&temp,vector<vector<int>>&ans,unordered_set<int>&st){
        if(temp.size()==nums.size()){
            ans.push_back(temp);
            return;
        }

        for(int i=0; i<nums.size();i++){
            if(st.find(nums[i])==st.end()){
                temp.push_back(nums[i]);
                st.insert(nums[i]);
                solve(nums,temp,ans,st);
                temp.pop_back();
                st.erase(nums[i]);
            }
        }
    }

    vector<vector<int>> permute(vector<int>& nums) {
        vector<int>temp;
        vector<vector<int>>ans;
        unordered_set<int>st;
        solve(nums,temp,ans,st);
        return ans;

    }
};

class Solution {
public:
    bool isSafe(int row, int col, vector<string>& board, int n) {
        // Check upper-left diagonal
        for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--)
            if (board[i][j] == 'Q') return false;

        // Check left side
        for (int j = col - 1; j >= 0; j--)
            if (board[row][j] == 'Q') return false;

        // Check lower-left diagonal
        for (int i = row + 1, j = col - 1; i < n && j >= 0; i++, j--)
            if (board[i][j] == 'Q') return false;

        return true;
    }

    void solve(int col, vector<string>& board, vector<vector<string>>& ans, int n) {
        if (col == n) {
            ans.push_back(board);
            return;
        }

        for (int row = 0; row < n; row++) {
            if (isSafe(row, col, board, n)) {
                board[row][col] = 'Q';
                solve(col + 1, board, ans, n);
                board[row][col] = '.';
            }
        }
    }

    vector<vector<string>> solveNQueens(int n) {
        vector<vector<string>> ans;
        vector<string> board(n, string(n, '.'));
        solve(0, board, ans, n);
        return ans;
    }
};

class Solution {
public:
    void solve(int col, vector<string>& board, vector<vector<string>>& ans,
               vector<int>& leftRow, vector<int>& upperDiagonal, vector<int>& lowerDiagonal, int n) {
        if (col == n) {
            ans.push_back(board);
            return;
        }

        for (int row = 0; row < n; row++) {
            if (leftRow[row] == 0 && lowerDiagonal[row + col] == 0 && upperDiagonal[n - 1 + col - row] == 0) {
                board[row][col] = 'Q';
                leftRow[row] = 1;
                lowerDiagonal[row + col] = 1;
                upperDiagonal[n - 1 + col - row] = 1;

                solve(col + 1, board, ans, leftRow, upperDiagonal, lowerDiagonal, n);

                board[row][col] = '.';
                leftRow[row] = 0;
                lowerDiagonal[row + col] = 0;
                upperDiagonal[n - 1 + col - row] = 0;
            }
        }
    }

    vector<vector<string>> solveNQueens(int n) {
        vector<vector<string>> ans;
        vector<string> board(n, string(n, '.'));
        vector<int> leftRow(n, 0), upperDiagonal(2 * n - 1, 0), lowerDiagonal(2 * n - 1, 0);
        solve(0, board, ans, leftRow, upperDiagonal, lowerDiagonal, n);
        return ans;
    }
};


class Solution {
public: 

    bool issafe(int row,int col, char ch,vector<vector<char>>&board){
        for(int i=0; i<9;i++){
            if(board[row][i]==ch){
                return false;//for col
            }

             if(board[i][col]==ch){
                return false;//for row
            }

            if(board[3*(row/3) + i/3][3*(col/3) + i%3]==ch){
                return false;//for particular compartment
            }
        }
        return true;
    }

    bool solve(vector<vector<char>>& board){
        for(int i=0; i<9;i++){
            for(int j=0;j<9;j++){
                if(board[i][j]=='.'){
                    for(char c='1';c<='9';c++){
                        if(issafe(i,j,c,board)){
                            board[i][j]=c;
                            if(solve(board))return true;
                            board[i][j]='.';
                    }
                    }
                    return false;
                }
             }
         }
         return true;
    }
    void solveSudoku(vector<vector<char>>& board) {

        solve(board);
        
    }
};

class Solution{
public: 
    bool issafe(unordered_map<int,vector<int>>&mpp,int node,int color,vector<int>&visited){
          for (int adj : mpp[node]) {
            if (visited[adj] == color) return false;
        }
        return true;
    }

    bool solve(unordered_map<int,vector<int>>&mpp,int i,int m,int n,vector<int>&visited){
        if(i==n){
           return true;
        }

        for(int color=0; color<m;color++){
            if(issafe(mpp,i,color,visited)){
                visited[i]=color;
                if(solve(mpp,i+1,m,n,visited))return true;
                visited[i]=-1;
            }
        }
        return false;
    }

    bool graphColoring(vector<vector<int> >& edges, int m, int n) {
    	//your code goes here
    
        vector<int>visited(n,-1);
        unordered_map<int,vector<int>>mpp;
         for (auto& edge : edges) {
            int from = edge[0], to = edge[1];
            mpp[from].push_back(to);
            mpp[to].push_back(from);
        }
        return solve(mpp, 0, m, n, visited);
    }
};

class Solution{
    public:
    int n;
    void solve(vector<vector<int>>&grid,int dx[],int dy[],string& ch,int row,int col,string temp,vector<string>&ans, vector<vector<int>>&visited){
        if(row==0 && col==0){
            reverse(temp.begin(),temp.end());
            ans.push_back(temp);
        }

        visited[row][col]=1;
        for(int i=0; i<4;i++){
            int nexti=row+dx[i];
            int nextj=col+dy[i];
            if(nexti>=0 && nextj>=0 && nexti<n && nextj<n && visited[nexti][nextj]==0 && grid[nexti][nextj]==1){

                solve(grid,dx,dy,ch,nexti,nextj,temp+ch[i],ans,visited);
                
            }

        }
        visited[row][col]=0;
    }
    vector<string> findPath(vector<vector<int> > &grid) {
        n= grid.size();
        if(grid[0][0]==0 || grid[n-1][n-1]==0)return {};
        //your code goes here

        int dx[4]={1,0,0,-1};
        int dy[4]={0,-1,1,0};
        string ch="DLRU";
        vector<string>ans;
        vector<vector<int>>visited(n,vector<int>(n,0));
        solve(grid,dx,dy,ch,n-1,n-1,"",ans,visited);
        return ans;
    }
};

# 🔍 Leetcode 540: Single Element in a Sorted Array

## ✅ Problem Statement

You are given a sorted array consisting of only integers where every element appears exactly twice, except for one element which appears exactly once.

Find that single element in **O(log n)** time and **O(1)** space.

---

## 🧠 Approach

We use **binary search** with the observation that:

- In a perfectly paired array, before the single element:
  - First instance of a pair appears at even index
  - Second instance at odd index
- After the single element, this pairing breaks

We search for the **first occurrence** where this pairing fails.

---

## 👨‍💻 Code (C++)

```cpp
class Solution {
public:
    int singleNonDuplicate(vector<int>& nums) {
        int n = nums.size();
        if (n == 1) return nums[0];

        if (nums[0] != nums[1]) return nums[0];
        if (nums[n - 1] != nums[n - 2]) return nums[n - 1];

        int l = 1;
        int h = n - 2;

        while (l <= h) {
            int mid = l + (h - l) / 2;

            // Check if nums[mid] is the unique element
            if (nums[mid] != nums[mid - 1] && nums[mid] != nums[mid + 1]) {
                return nums[mid];
            }

            // Pair is in correct order → unique is on the right
            if ((mid % 2 == 0 && nums[mid] == nums[mid + 1]) ||
                (mid % 2 == 1 && nums[mid] == nums[mid - 1])) {
                l = mid + 1;
            } else {
                h = mid - 1;
            }
        }

        return -1; // Should never reach here
    }
};


# 🔍 Leetcode 33: Search in Rotated Sorted Array

## ✅ Problem Statement

You are given an integer array `nums` sorted in ascending order (with **distinct** values), but it has been **rotated at an unknown pivot**.

You are also given a target value. Return the **index** of the target if it is in the array; otherwise, return `-1`.

You must write an algorithm with **O(log n)** runtime complexity.

---

## 🧠 Approach

We apply **binary search** with an additional check to determine which side of the array is sorted.

At each step:
- Check if the **left half** is sorted.
- If target lies in the **sorted half**, adjust `l` and `r` accordingly.
- Otherwise, search in the other half.

---

## 👨‍💻 Code (C++)

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int n = nums.size();
        int l = 0, r = n - 1;

        while (l <= r) {
            int mid = l + (r - l) / 2;

            if (nums[mid] == target) return mid;

            // Check if left half is sorted
            if (nums[l] <= nums[mid]) {
                if (nums[l] <= target && target <= nums[mid]) {
                    r = mid - 1;
                } else {
                    l = mid + 1;
                }
            }
            // Else, right half is sorted
            else {
                if (nums[mid] <= target && target <= nums[r]) {
                    l = mid + 1;
                } else {
                    r = mid - 1;
                }
            }
        }

        return -1;
    }
};


# 📚 Book Allocation Problem

## ✅ Problem Statement

You are given:
- An array `nums[]` of integers where each element represents the number of pages in a book.
- An integer `m` representing the number of students.

The task is to allocate books such that:
1. Each student gets **at least one book**.
2. Each book is allocated to exactly one student.
3. The **maximum number of pages assigned to any student** is minimized.

Return this **minimum possible maximum pages**.

If the number of books is less than the number of students, return `-1`.

---

## 🧠 Approach

We use **Binary Search on the Answer**:

1. **Range of Search:**
   - Lower bound `l` = `max(nums)` → A student must read at least the largest book.
   - Upper bound `h` = `sum(nums)` → One student reads all books.

2. **Feasibility Function `func()`**:
   - Given a maximum allowed page limit `pages`, we check if we can allocate books to **at most `m` students** without exceeding `pages` for any student.
   - If possible, we try a smaller `pages` (move left).
   - If not possible, we increase `pages` (move right).

3. **Goal:**
   - Find the smallest `pages` such that allocation is possible.

---

## 👨‍💻 Code (C++)

```cpp
class Solution {
public:
    int func(vector<int> &nums, int pages, int m) {
        int cnt = 1; // number of students
        int last = nums[0]; // pages for current student
        
        for (int i = 1; i < nums.size(); i++) {
            if (last + nums[i] > pages) {
                cnt++;
                last = 0;
            }
            last += nums[i];
        }
        return cnt <= m;
    }

    int findPages(vector<int> &nums, int m) {
        if (nums.size() < m) return -1; // Not enough books

        int l = *max_element(nums.begin(), nums.end());
        int h = accumulate(nums.begin(), nums.end(), 0);
        int ans = 0;

        while (l <= h) {
            int mid = l + (h - l) / 2;
            if (func(nums, mid, m)) {
                ans = mid;
                h = mid - 1;
            } else {
                l = mid + 1;
            }
        }
        return ans;
    }
};



class Solution {
public:
    int func(vector<int> &nums, int dist,int k){
        int last=0;
        int cnt=1;
        for(int i=1; i<nums.size();i++){
            if(nums[i]-nums[last]>=dist){
                cnt++;
                last=i;
            }
        }
        if(k<=cnt)return 1;
        return 0;
    }
    int aggressiveCows(vector<int> &nums, int k) {
        sort(nums.begin(),nums.end());
        int ans=0;
        int l=1; int h=nums[nums.size()-1]-nums[0];
        while(l<=h){
            int mid=l+(h-l)/2;
            if(func(nums,mid,k)){
                ans=mid;
                l=mid+1;
            }else{
                h=mid-1;
            }
        }
        return ans;
    }
};
