
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


## 🔁 Palindrome Linked List

### 🔗 Problem
[LeetCode 234 - Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/)

---

### 🧠 Summary

To check if a singly linked list is a palindrome:

1. **Find the middle** of the list using the slow and fast pointer technique.  
   - In case of an **even-length** list, the `slow` pointer will point to the **first** of the two middle nodes.
2. **Reverse the second half** of the list starting from the node after `slow`.
3. **Compare** the first half and the reversed second half.
4. (Optional) Restore the reversed half to keep the list unchanged.

If all corresponding nodes match, the list is a palindrome.

---

### 💻 Code (C++)

```cpp
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        if (!head || !head->next) return true;

        // Step 1: Find the middle
        ListNode* slow = head;
        ListNode* fast = head;
        while (fast->next && fast->next->next) {
            slow = slow->next;
            fast = fast->next->next;
        }

        // Step 2: Reverse second half
        ListNode* prev = nullptr;
        ListNode* curr = slow->next;
        while (curr) {
            ListNode* nextt = curr->next;
            curr->next = prev;
            prev = curr;
            curr = nextt;
        }

        // Step 3: Compare both halves
        ListNode* first = head;
        ListNode* second = prev;
        while (second) {
            if (first->val != second->val) return false;
            first = first->next;
            second = second->next;
        }

        return true;
    }
};

# Flatten a Linked List

## 🧩 Problem Statement

You are given a linked list where each node has two pointers:
- `next` – points to the next node in the main list
- `bottom` – points to a sub-linked list

The `next` pointer connects nodes at the top level, and the `bottom` pointer connects nodes of the sub-linked list.

Your task is to **flatten** the list such that all the nodes appear in a single-level linked list sorted in ascending order. Only use the `bottom` pointer to form the final flattened list.

---

## 🧠 Intuition

This problem is similar to merging **K sorted linked lists**. Each node connected by `next` can be treated as a head of a separate sorted list (connected by `bottom`). To flatten:

1. Recursively flatten the list starting from the right (`next` pointer).
2. Use a **merge** function to combine the current list with the flattened list from the right.

Think of it like reducing the problem one `next` node at a time, and merging step-by-step.

---

## 🔍 Approach

1. If the list is empty or has only one node, return it.
2. Recursively flatten the `next` list.
3. Merge the current node's bottom list with the result of the flattened `next` list using a helper merge function.
4. Return the merged list.

---

## ✅ Code

```cpp
struct Node {
    int data;
    Node* next;
    Node* bottom;
};

Node* merge(Node* a, Node* b) {
    if (!a) return b;
    if (!b) return a;

    Node* result;

    if (a->data < b->data) {
        result = a;
        result->bottom = merge(a->bottom, b);
    } else {
        result = b;
        result->bottom = merge(a, b->bottom);
    }
    return result;
}

Node* flatten(Node* root) {
    if (!root || !root->next) return root;

    // Recursively flatten the next list
    root->next = flatten(root->next);

    // Merge this list with the flattened next list
    root = merge(root, root->next);

    return root;
}


## 🔗 21. Merge Two Sorted Lists

### ✅ Problem Summary:
Given two sorted linked lists `list1` and `list2`, merge them into one sorted linked list and return the head of the new list.

---

### 🧠 Intuition:

- Use two pointers `l1` and `l2` starting from the heads of each list.
- Always ensure `l1` points to the smaller-valued node.
- Traverse through both lists and adjust the `next` pointers to stitch them in sorted order.
- Swap pointers when needed to maintain the invariant that `l1` is always smaller.

This is a **pointer-based merge** (like in merge sort), without creating a new list.

---

### 🧪 Edge Cases:
- Either list is `nullptr` → return the other list.
- One or both lists are empty.

---

### 💻 Code (C++)

```cpp
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        ListNode* l1 = list1;
        ListNode* l2 = list2;
        if (l1 == nullptr) return l2;
        if (l2 == nullptr) return l1;

        // Ensure l1 starts with the smaller value
        if (l1->val > l2->val) swap(l1, l2);

        ListNode* res = l1;

        while (l1 != nullptr && l2 != nullptr) {
            ListNode* temp = nullptr;
            while (l1 != nullptr && l1->val <= l2->val) {
                temp = l1;
                l1 = l1->next;
            }
            temp->next = l2;
            swap(l1, l2); // Make sure l1 always points to the smaller node
        }

        return res;
    }
};

💡 Approach
Edge Case: If the list is empty or has only one node, or k == 0, return head.

Count Length: Traverse the list to count the number of nodes.

Link Last Node to Head: Make the list circular.

Find New Head: Move (length - k % length) steps to find the new tail.

Break the Circle: Set newTail->next = NULL.

class Solution {
public:
    ListNode* rotateRight(ListNode* head, int k) {
        if (!head || !head->next || k == 0) return head;
        
        // Count length and connect tail to head
        ListNode* temp = head;
        int len = 1;
        while (temp->next) {
            len++;
            temp = temp->next;
        }
        temp->next = head; // make it circular

        k = k % len;
        int skip = len - k;

        ListNode* newTail = head;
        for (int i = 1; i < skip; i++) {
            newTail = newTail->next;
        }

        ListNode* newHead = newTail->next;
        newTail->next = NULL;

        return newHead;
    }
};
