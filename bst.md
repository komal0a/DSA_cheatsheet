class Solution {
public:
    TreeNode* searchBST(TreeNode* root, int val) {
        while(root){
            if(root->val==val)return root;

            if(root->val<val){
                root=root->right;
            }else{
                root=root->left;
            }
        }  
        return nullptr;
    }
};


class Solution {
public:
    Node* connect(Node* root) {
        // If the tree is empty, there's nothing to do.
        if (!root) {
            return nullptr;
        }

        // 'leftmost' will be the starting node of each level.
        Node* leftmost = root;

        // We iterate as long as the leftmost node has a left child.
        // Since it's a perfect binary tree, if it has a left child, it has a right one too.
        while (leftmost->left) {
            // 'current' will traverse the nodes of the current level.
            Node* current = leftmost;

            // Traverse the current level and connect the children of each node.
            while (current) {
                // 1. Connect the left child to the right child of the same parent.
                current->left->next = current->right;

                // 2. Connect the right child to the left child of the parent's sibling.
                // This is only possible if the current node has a 'next' pointer.
                if (current->next) {
                    current->right->next = current->next->left;
                }

                // Move to the next node in the current level.
                current = current->next;
            }
            
            // Move to the start of the next level.
            leftmost = leftmost->left;
        }

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
    TreeNode* solve(vector<int>& nums,int start,int end){
        if(start>end)return nullptr;
        int mid = start + (end - start) / 2;
        TreeNode* root=new TreeNode(nums[mid]);
        root->left=solve(nums,start,mid-1);
        root->right=solve(nums,mid+1,end);
        return root;
        }
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        if(nums.empty())return nullptr;
        int n= nums.size();
        return solve(nums,0,n-1);
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
    bool solve(TreeNode* root,long long mini,long long maxi){
        if(!root)return true;

        if(root->val <= mini || root->val >= maxi)return false;
        bool  lh=solve(root->left,mini,root->val);
        bool  rh=solve(root->right,root->val,maxi);

        return lh && rh;
    }
    bool isValidBST(TreeNode* root) {
     return solve(root,LLONG_MIN,LLONG_MAX);   
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
        if(root==nullptr)return nullptr;
        int cur=root->val;
        if(p->val<cur && q->val<cur){
           return lowestCommonAncestor(root->left,p,q);
        }
        else if(p->val>cur && q->val>cur)   {
          return  lowestCommonAncestor(root->right,p,q);
        }   
        return root;
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
class BSTIterator {
    private: stack<TreeNode*>st;
public:
    BSTIterator(TreeNode* root) {
        pushAll(root);
    }
    
    int next() {
        TreeNode* node=st.top();
        st.pop();
        pushAll(node->right);
        return node->val;
    }
    
    bool hasNext() {
     return !st.empty();   
    }

    private: 
    void pushAll(TreeNode* node){
        while(node){
            st.push(node);
            node=node->left;
        }
    }
};

/**
 * Your BSTIterator object will be instantiated and called as such:
 * BSTIterator* obj = new BSTIterator(root);
 * int param_1 = obj->next();
 * bool param_2 = obj->hasNext();

 */


/**
 * Definition for a binary tree node.
 * class TreeNode {
 *     int data;
 *     TreeNode *left;
 *     TreeNode *right;
 *      TreeNode(int val) : data(val) , left(nullptr) , right(nullptr) {}
 * };
 **/

class Solution{	
	public:
		vector<int> floorCeilOfBST(TreeNode* root,int key){
			//your code goes here
            vector<int>ans(2,-1);
          int ceil=-1;
          int floor=-1;
            while(root){
                if(root->data==key){
                   floor=ceil=key;
                   break;
                }
            if(root->data<key){
                floor=root->data;
                root=root->right;
            }
                else{
                   ceil= root->data;
                    root=root->left;
                }
            }

            ans[0]=floor;
            ans[1]=ceil;
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
 class BSTiterator{
    private:
    stack<TreeNode*>st;
    bool before=true;

    public:
    BSTiterator(TreeNode* root,bool isbefore){
        before=isbefore;
        pushAll(root);
    }
    bool hasnext(){
        return !st.empty();
    }

    int next(){
        TreeNode* node= st.top();
        st.pop();

        if(before)pushAll(node->left);
        else pushAll(node->right);

        return node->val;
    }

    void pushAll(TreeNode* node){
        while(node){
            st.push(node);
            if(before){
                node=node->right;
            }else{
                node=node->left;
            }
        }
    }
 };
class Solution {
public:
    bool findTarget(TreeNode* root, int k) {
        if(!root)return false;
        BSTiterator l(root,false);//increasing
        BSTiterator r(root,true);//decreasing

        int i = l.next();
        int j = r.next();

        while (i < j) {
            int sum = i + j;
            if (sum == k) return true;
            else if(sum< k)i=l.next();//increase
            else j=r.next();//decrease
            }

        return false;
        
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
    int kthSmallest(TreeNode* root, int k) {
        int counter = 0;
        int res = INT_MIN;
        TreeNode* curr = root;

        //using morris inorder traversal
        while (curr != nullptr) {
            if (curr->left == nullptr) {
                counter++;
                if (counter == k)
                    res = curr->val;
                    curr = curr->right;
            } else {
                TreeNode* pre = curr->left;
                while (pre->right != nullptr && pre->right != curr) {
                    pre = pre->right;
                }

                if (pre->right == nullptr) {
                    pre->right = curr;
                    curr = curr->left;
                } else {
                    pre->right = nullptr;
                    counter++;
                    if (counter == k)
                     res = curr->val;
                    curr = curr->right;
                }
            }
        }

        return res;
    }

};

class Solution {
public:
    int count = 0;
    int ans = -1;

    void reverseInorder(TreeNode* root, int k) {
        if (!root || count >= k) return;

        reverseInorder(root->right, k);

        count++;
        if (count == k) {
            ans = root->val;
            return;
        }

        reverseInorder(root->left, k);
    }

    int kthLargest(TreeNode* root, int k) {
        reverseInorder(root, k);
        return ans;
    }
};

