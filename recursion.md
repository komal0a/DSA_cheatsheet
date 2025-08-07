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
