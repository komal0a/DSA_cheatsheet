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
