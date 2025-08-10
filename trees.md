/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int data;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int val) : data(val), left(nullptr), right(nullptr) {}
 * };
 **/

class Solution {
public:
    vector<int> rightSideView(TreeNode* root) {
        // Handle empty tree case
        if (!root) return {};

        // Map to store the last (rightmost) node at each level
        map<int, int> mp; // level -> node value

        // BFS queue: stores (node, level)
        queue<pair<TreeNode*, int>> qt;
        qt.push({root, 0});
        mp[0] = root->data;

        while (!qt.empty()) {
            TreeNode* node = qt.front().first;
            int level = qt.front().second;
            qt.pop();

            // Update rightmost node for this level
            mp[level] = node->data;

            // Push left and right children into queue
            if (node->left)  qt.push({node->left,  level + 1});
            if (node->right) qt.push({node->right, level + 1});
        }

        // Collect results in order of levels
        vector<int> ans;
        for (auto it : mp) {
            ans.push_back(it.second);
        }

        return ans;
    }
};

/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int data;
 *     TreeNode *left;
 *     TreeNode *right;
 *      TreeNode(int val) : data(val) , left(nullptr) , right(nullptr) {}
 * };
 **/

class Solution {
  public:
    vector <int> bottomView(TreeNode *root){
    	//your code goes here
       map<int,int>mpp;//line,val
        queue<pair<TreeNode*,int>>qt;//node,vertical line

        qt.push({root,0});

        while(!qt.empty()){
            TreeNode* node= qt.front().first;
            int line= qt.front().second;
            qt.pop();

             mpp[line]=node->data;

            if(node->left)qt.push({node->left,line-1});
            if(node->right)qt.push({node->right,line+1});
        }

        vector<int>ans;
        for(auto& [line,val]:mpp){
            ans.push_back(val);
        }
        return ans;
    }
};


/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int data;
 *     TreeNode *left;
 *     TreeNode *right;
 *      TreeNode(int val) : data(val) , left(nullptr) , right(nullptr) {}
 * };
 **/

class Solution{
    public:
    vector<int> topView(TreeNode *root){
        //your code goes here
        map<int,int>mpp;//line,val
        queue<pair<TreeNode*,int>>qt;//node,vertical line

        qt.push({root,0});

        while(!qt.empty()){
            TreeNode* node= qt.front().first;
            int line= qt.front().second;
            qt.pop();

            if(mpp.find(line)==mpp.end()){
                mpp[line]=node->data;
            }

            if(node->left)qt.push({node->left,line-1});
            if(node->right)qt.push({node->right,line+1});
        }

        vector<int>ans;
        for(auto& [line,val]:mpp){
            ans.push_back(val);
        }
        return ans;
    }
};

/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> verticalTraversal(TreeNode* root) {
        map<int,map<int,multiset<int>>>nodes;//vertical,level,nodes---> all are sorted
        queue<pair<TreeNode*,pair<int,int>>>todo;//{node,vertical,level}

        todo.push({root,{0,0}});

        while(!todo.empty()){
            TreeNode* temp=todo.front().first;
            int x= todo.front().second.first;
            int y= todo.front().second.second;
            todo.pop();

            nodes[x][y].insert(temp->val);

            if(temp->left){
                todo.push({temp->left,{x-1,y+1}});
            }
            if(temp->right){
                todo.push({temp->right,{x+1,y+1}});
            }
        }

        vector<vector<int>>ans;
        for(auto it:nodes){
            vector<int>col;
            for(auto p:it.second){
                col.insert(col.end(),p.second.begin(),p.second.end());

            }
            ans.push_back(col);
            
        }
        return ans;
    }
};

/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int data;
 *     TreeNode *left;
 *     TreeNode *right;
 *      TreeNode(int val) : data(val) , left(nullptr) , right(nullptr) {}
 * };
 **/

class Solution {
public:
    // Helper function to perform DFS and track root-to-leaf paths
    void solve(TreeNode* root, vector<vector<int>>& ans, vector<int>& ds) {
        // Base case: if node is null, return
        if (root == nullptr) return;

        // Step 1: Process current node
        ds.push_back(root->data);

        // Step 2: Check if it's a leaf node
        if (root->left == nullptr && root->right == nullptr) {
            ans.push_back(ds);
        }

        // Step 3: Recurse on left and right subtrees
        solve(root->left, ans, ds);
        solve(root->right, ans, ds);

        // Step 4: Backtrack (remove current node before returning)
        ds.pop_back();
    }

    vector<vector<int>> allRootToLeaf(TreeNode* root) {
        vector<vector<int>> ans;
        vector<int> ds; // Temporary path storage
        solve(root, ans, ds);
        return ans;
    }
};
🛠 Time & Space Complexity
Time Complexity: O(N) — Each node is visited once.

Space Complexity: O(H) — Height of tree for recursion stack + path storage.

# 📝 Problem: Path from Root to a Given Node in a Binary Tree

## 📄 Problem Statement
Given the root of a binary tree and a target value, return the path from the root to the target node as a list of integers.  
If the target does not exist in the tree, return an empty list.

---

## 💡 Approach
We perform a **DFS traversal** starting from the root, keeping track of the path in a vector.  
At each step:
1. Add the current node to the path.
2. If it matches the target, return `true`.
3. Otherwise, recursively search in the left and right subtrees.
4. If the target is not found in either branch, **backtrack** by removing the last node from the path.

---

## ⏳ Time Complexity
- **O(N)** — In the worst case, we may visit all nodes to find the target.

## 📦 Space Complexity
- **O(H)** — Where `H` is the height of the tree (recursion stack space).
- Path vector stores at most `H` nodes at a time.

---

## 💻 Code Implementation

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int data;
 *     TreeNode *left;
 *     TreeNode *right;
 *      TreeNode(int val) : data(val) , left(nullptr) , right(nullptr) {}
 * };
 **/

class Solution {
public:
    bool findPath(TreeNode* root, vector<int>& path, int target) {
        if (!root) return false;

        // Add current node to path
        path.push_back(root->data);

        // If current node is target
        if (root->data == target) return true;

        // Recurse left or right
        if (findPath(root->left, path, target) || findPath(root->right, path, target))
            return true;

        // Backtrack if target not found in this path
        path.pop_back();
        return false;
    }

    vector<int> pathToNode(TreeNode* root, int target) {
        vector<int> path;
        findPath(root, path, target);
        return path; // Empty if target not found
    }
};
