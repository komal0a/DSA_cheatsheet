
## 🔄 Reverse Linked List

### ✅ Problem Statement
Given the head of a singly linked list, reverse the list and return the head of the reversed list.
 */
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* prev = nullptr;
        ListNode* cur = head;

        while (cur != nullptr) {
            ListNode* nextt = cur->next;
            cur->next = prev;
            prev = cur;
            cur = nextt;
        }

        return prev;
    }
};

 */
class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        ListNode* slow=head;
        ListNode* fast=head;
        while(fast && fast->next != nullptr){
            slow=slow->next;
            fast=fast->next->next;
        }
        return slow;
    }

    ✅ To get the first middle, just change initialization of fast:
cpp
Copy
Edit
ListNode* slow = head;
ListNode* fast = head->next;

while (fast && fast->next) {
    slow = slow->next;
    fast = fast->next->next;
}
return slow;
}

## ❌ Remove N-th Node from End of List

### 🔗 Problem
[LeetCode 19 - Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)  
Given the head of a linked list, remove the n-th node from the end of the list and return its head.

---

### 📘 Example

**Input:**  
`head = [1, 2, 3, 4, 5], n = 2`  
**Output:**  
`[1, 2, 3, 5]`  

---

### 🧠 Approach: Two Pointer Technique (Fast and Slow)

1. Create a dummy node pointing to head (to handle edge cases smoothly).
2. Move `fast` pointer `n` steps ahead.
3. Move both `fast` and `slow` one step at a time until `fast->next` becomes `NULL`.
4. Now `slow` is just before the node to be removed. Do `slow->next = slow->next->next`.

---

### ⚠️ Edge Case

If the list has only one node and `n = 1`  
→ we must remove the head node.

**Input:**  
`head = [1], n = 1`  
**Output:**  
`[]`  

To handle this, we use a dummy node before `head`. This helps avoid null pointer errors and allows us to delete the head safely.

---

### ✅ Code (C++)

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */

class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummy = new ListNode(0, head);
        ListNode* fast = dummy;
        ListNode* slow = dummy;

        // Move fast n steps ahead
        while (n--) {
            fast = fast->next;
        }

        // Move fast to the end, maintaining the gap
        while (fast->next != nullptr) {
            fast = fast->next;
            slow = slow->next;
        }

        // Skip the node to be deleted
        slow->next = slow->next->next;

        return dummy->next;
    }
};
## ❌ Delete Node in a Linked List

### 🔗 Problem  
[LeetCode 237 – Delete Node in a Linked List](https://leetcode.com/problems/delete-node-in-a-linked-list/)  

You are given a **node** in a singly linked list (not the head), and you are required to delete it.  
You **cannot access the head**, and the node is **guaranteed not to be the last node**.

---

### 📘 Example

**Input:**  
`head = [4, 5, 1, 9]`, node = node with value `5`  
**Output:**  
`[4, 1, 9]`

---

### 🧠 Approach

Since you **don't have access to the previous node**, you can't adjust its `next` pointer.

So instead:
- Copy the **value** of the next node into the current node.
- Skip the next node by changing `node->next`.

In short, you "overwrite" the current node with the next one.

---

### ✅ Code (C++)

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(nullptr) {}
 * };
 */

class Solution {
public:
    void deleteNode(ListNode* node) {
        node->val = node->next->val;
        node->next = node->next->next;
    }
};

## 🔗 Intersection of Two Linked Lists

### 🧠 Summary

We use two pointers, `t1` and `t2`, initialized at the heads of both lists.  
Each pointer traverses its list and then switches to the other list when it reaches the end.  
This ensures both pointers cover equal distance. They will meet at the intersection node, or both will become `nullptr` if no intersection exists.

- Time Complexity: **O(n + m)**
- Space Complexity: **O(1)**
- Handles different list lengths and no-intersection cases smoothly.
