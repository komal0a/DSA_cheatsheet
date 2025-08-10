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

# 📝 Problem: Maximum Width of Binary Tree

## 📄 Problem Statement
Given the root of a binary tree, return the **maximum width** of the given tree.  
The **maximum width** is the maximum number of nodes at any level, **counting the null nodes between non-null nodes**.

The width of a level is defined as the length between the **leftmost** and **rightmost** non-null nodes (inclusive of null positions).

---

## 💡 Intuition
We can think of the binary tree as being **indexed** like a binary heap:
- Root has index `0`
- Left child = `2 * index + 1`
- Right child = `2 * index + 2`

If we know the **index of the first and last node** at each level,  
we can calculate that level's width as:

⚠ **Overflow Prevention**:  
Since indices grow exponentially for deep trees, we store them as `unsigned long long` and normalize each level by subtracting the minimum index of that level (`mini`).

---

## ⏳ Time Complexity
- **O(N)** — Each node is visited once.

## 📦 Space Complexity
- **O(N)** — Queue for BFS traversal.

---

## 💻 Code Implementation

```cpp
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
    int widthOfBinaryTree(TreeNode* root) {
        if (root == nullptr) return 0;

        // BFS queue: stores {node, index}
        queue<pair<TreeNode*, unsigned long long>> qt;
        qt.push({root, 0});
        int maxi = 0;

        while (!qt.empty()) {
            int size = qt.size();
            unsigned long long mini = qt.front().second; // normalize indices
            unsigned long long first = 0, last = 0;

            for (int i = 0; i < size; i++) {
                TreeNode* node = qt.front().first;
                unsigned long long val = qt.front().second - mini;
                qt.pop();

                if (i == 0) first = val;
                if (i == size - 1) last = val;

                if (node->left)  qt.push({node->left, val * 2 + 1});
                if (node->right) qt.push({node->right, val * 2 + 2});
            }

            maxi = max(maxi, static_cast<int>(last - first + 1));
        }

        return maxi;
    }
};

# Inorder Traversal of a Binary Tree

Inorder traversal visits nodes in the order: **Left → Root → Right**

---

## 1. Recursive Inorder Traversal

```cpp
void inorderRecursive(TreeNode* root, vector<int>& result) {
    if (!root) return;               // Base case: if node is null, return

    inorderRecursive(root->left, result);  // Traverse left subtree first
    result.push_back(root->val);            // Visit current node (root)
    inorderRecursive(root->right, result); // Traverse right subtree last
}


vector<int> inorderIterative(TreeNode* root) {
    vector<int> result;
    stack<TreeNode*> st;
    TreeNode* curr = root;

    while (curr != nullptr || !st.empty()) {
        // Go as left as possible, pushing all nodes on the stack
        while (curr != nullptr) {
            st.push(curr);
            curr = curr->left;
        }
        
        // Current is nullptr here, so process the top node from the stack
        curr = st.top();
        st.pop();

        result.push_back(curr->val);  // Visit the node

        // Move to the right subtree to continue inorder traversal
        curr = curr->right;
    }
    return result;
}

vector<int> inorderMorris(TreeNode* root) {
    vector<int> result;
    TreeNode* curr = root;

    while (curr != nullptr) {
        if (curr->left == nullptr) {
            // If no left child, visit node and go right
            result.push_back(curr->val);
            curr = curr->right;
        } else {
            // Find the inorder predecessor (rightmost node in left subtree)
            TreeNode* prev = curr->left;
            while (prev->right != nullptr && prev->right != curr) {
                prev = prev->right;
            }

            if (prev->right == nullptr) {
                // Thread the tree: link predecessor's right to current node
                prev->right = curr;
                curr = curr->left;
            } else {
                // Left subtree already visited, revert changes (remove thread)
                prev->right = nullptr;
                result.push_back(curr->val);  // Visit current node
                curr = curr->right;
            }
        }
    }

    return result;
}
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
    int maxDepth(TreeNode* root) {
        if(root==nullptr)return 0;

        int lh=maxDepth(root->left);
        int rh=maxDepth(root->right);

        return 1+max(lh,rh);
    }
};

# Diameter of Binary Tree

**Problem:**  
Find the length of the diameter of a binary tree. The diameter is the length of the longest path between any two nodes in the tree. This path may or may not pass through the root.

---

### Intuition:

- The diameter at any node is the sum of the height of its left subtree and the height of its right subtree.
- To find the diameter, recursively compute the height of left and right subtrees.
- Update the maximum diameter at every node as the sum of left and right heights.
- Return the maximum diameter found.

---

### Code (C++)

```cpp
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
    int solve(TreeNode* root, int &maxi){
        if(root == nullptr) return 0;

        int lh = solve(root->left, maxi);   // Height of left subtree
        int rh = solve(root->right, maxi);  // Height of right subtree

        // Update diameter at this node (left height + right height)
        maxi = max(maxi, lh + rh);

        // Return height of current node
        return 1 + max(lh, rh);
    }

    int diameterOfBinaryTree(TreeNode* root) {
        int maxi = 0;
        solve(root, maxi);
        return maxi;
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
    int solve(TreeNode* root){
        if(root==nullptr)return 0;

        int lh=solve(root->left);
        if(lh==-1)return -1;

         int rh=solve(root->right);
        if(rh==-1)return -1;

        if(abs(lh-rh)>1)return -1;

        return max(lh,rh)+1;

    }
    bool isBalanced(TreeNode* root) {
        return solve(root) != -1;        
    }
};

/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* solve(TreeNode* root, TreeNode* p, TreeNode* q){
        // if(root==nullptr)return null;

        if(root==nullptr|| root==p || root==q)return root;

        TreeNode* lh=solve(root->left,p,q);
         TreeNode* rh=solve(root->right,p,q);

        if(lh==nullptr){
            return rh;
        }else if(rh==nullptr){
            return lh;
        }else{
            return root;
        }
    }
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
       return solve(root,p,q);
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
    bool solve(TreeNode* p, TreeNode* q){
        if(p==nullptr && q==nullptr)return true;
        if(p==nullptr || q==nullptr)return false;
        if(p->val != q->val)return false;
            
        int lh=solve(p->left,q->left);
        int rh= solve(p->right, q->right);

        return lh && rh;

    }
    bool isSameTree(TreeNode* p, TreeNode* q) {
     return solve(p,q);
         
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
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        vector<vector<int>>ans;
        if(root==nullptr)return ans;
        vector<int>ds;
        queue<TreeNode*>qt;

        bool flag=false;
        qt.push(root);  
        while(!qt.empty()){
            int size= qt.size();
            for(int i=0; i<size ;i++){
             TreeNode* node=qt.front();
            qt.pop();

            ds.push_back(node->val);
            if(node->left)qt.push(node->left);
            if(node->right)qt.push(node->right);
            }
            if(flag)reverse(ds.begin(),ds.end());
            ans.push_back(ds);
            ds.clear();
            flag=!flag;
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
    bool leaf_node(TreeNode* node){
        return !node->left && !node->right;
    }
    void left(TreeNode* root, vector<int>&ans){
        TreeNode* cur= root->left;
        while(cur){

        if(!leaf_node(cur)){
            ans.push_back(cur->data);

        }
            if(cur->left){

            cur=cur->left;
            }else{

            cur=cur->right;
            }
        }
    }

    void leaf(TreeNode* root, vector<int>&ans){
        if(leaf_node(root)){
            ans.push_back(root->data);
            return;
        }
        if(root->left)leaf(root->left,ans);
         if(root->right)leaf(root->right,ans);
    }

     void right(TreeNode* root, vector<int>&ans){
        TreeNode* cur= root->right;
        vector<int>temp;
        while(cur){
        if(!leaf_node(cur)){
            temp.push_back(cur->data);

        }
            if(cur->right){

            cur=cur->right;
            }else{

            cur=cur->left;
            }
        
        }
        reverse(temp.begin(),temp.end());
        for(int it:temp){
        ans.push_back(it);
        }
    }

    vector <int> boundary(TreeNode* root){
    	//your code goes here
        vector<int>ans;
        if(root==nullptr)return ans;

        if(!(leaf_node(root))){
            ans.push_back(root->data);
        }
        
        left(root,ans);
        leaf(root,ans);
        right(root,ans);

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
    int solve(TreeNode* root, int& maxi){
        if(root==nullptr)return 0;

        int lh=solve(root->left,maxi);
        int rh= solve(root->right,maxi);

        if(lh<0)lh=0;
        if(rh<0)rh=0;

        maxi=max(maxi,lh+rh+root->val);

        return root->val+max(lh,rh);
    }
    int maxPathSum(TreeNode* root) {
        int maxi= INT_MIN;
        solve(root,maxi);
        return maxi;
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
    bool solve(TreeNode* side1,TreeNode* side2){
        if(side1==nullptr && side2 == nullptr)return true;
        if(side1==nullptr || side2==nullptr)return false;

        if(side1->val !=side2->val)return false;

        int lh= solve(side1->left,side2->right);
        int rh= solve(side1->right,side2->left);

        return lh && rh;
    }
    bool isSymmetric(TreeNode* root) {
        if(root==nullptr)return true;
        return solve(root->left,root->right);

    }
};

/* class TreeNode {
       int val;
       TreeNode *left, *right;
       TreeNode(int x) : val(x), left(NULL), right(NULL) {}
   };
*/

class Solution {
public:
    bool solve(TreeNode* root){
      if (!root || (!root->left && !root->right)) return true;

       bool ls= solve(root->left);
       bool rs= solve(root->right);

       if(!rs || !ls)return false;
        
        int leftVal = root->left ? root->left->val : 0;
        int rightVal = root->right ? root->right->val : 0;

        // Check if current node value equals sum of left and right children
        if (root->val != leftVal + rightVal) return false;

        return true;

    }
    bool checkChildrenSum(TreeNode* root) {
        // Your code goes here
        return solve(root);
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
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        unordered_map<int,int>mpp;

        for(int i=0; i<inorder.size();i++){
            mpp[inorder[i]]=i;
        }

        TreeNode* root=solve(preorder,0,preorder.size()-1,inorder,0,inorder.size()-1,mpp);
        return root; 
    }

    TreeNode* solve(vector<int>& preorder,int prestart, int preend, vector<int>& inorder,int instart,int inend,unordered_map<int,int>&mpp){

        if(prestart>preend || instart>inend)return nullptr;

        int preroot=preorder[prestart];
        int inroot=mpp[preroot];
        int size=inroot-instart;

        TreeNode* root= new TreeNode(preroot);
        root->left=solve(preorder,prestart+1,prestart+size,inorder,instart,inroot-1,mpp);
        root->right=solve(preorder,prestart+size+1,preend,inorder,inroot+1,inend,mpp);

        return root;
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
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        unordered_map<int,int>mpp;

        for(int i=0; i<inorder.size();i++){
            mpp[inorder[i]]=i;
        }

        TreeNode* root=solve(postorder,0,postorder.size()-1,inorder,0,inorder.size()-1,mpp);
        return root; 
    }

     TreeNode* solve(vector<int>& postorder,int poststart, int postend, vector<int>& inorder,int instart,int inend,unordered_map<int,int>&mpp){

        if(poststart>postend || instart>inend)return nullptr;

        int postroot=postorder[postend];
        int inroot=mpp[postroot];
        int size=inroot-instart;

        TreeNode* root= new TreeNode(postroot);
        root->left=solve(postorder,poststart,poststart+size-1,inorder,instart,inroot-1,mpp);
        root->right=solve(postorder,poststart+size,postend-1,inorder,inroot+1,inend,mpp);

        return root;
    }
};
