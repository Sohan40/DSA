# 🧾 Problem: [Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal)

## 📝 Description
Inorder traversal is a depth-first traversal method for binary trees where we visit nodes in the order: Left subtree → Root → Right subtree. This traversal is particularly useful for binary search trees (BST) as it visits nodes in ascending order.

## 🎯 Applications
- Validating if a tree is a BST
- Getting elements in sorted order from a BST
- Expression tree evaluation
- Converting between different tree representations

## 🧠 Brute Force Approach:
- Use recursion to traverse the tree.
- Time Complexity: O(n)
- Space Complexity: O(h) where h is the height of the tree (due to recursion stack)

### 💻 Brute Force Code:
```cpp
class Solution {
private:
    void inorderHelper(TreeNode* root, vector<int>& res) {
        if (!root) return;
        
        // Process left subtree
        inorderHelper(root->left, res);
        
        // Process current node
        res.push_back(root->val);
        
        // Process right subtree
        inorderHelper(root->right, res);
    }
    
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> res;
        inorderHelper(root, res);
        return res;
    }
};
```

## 💡 Optimal Approach:
- Use an explicit stack for iterative inorder traversal
- Advantages:
  - No recursion stack overhead
  - More memory efficient for very deep trees
  - Easier to understand the flow of execution

### 💻 Optimal Code:
```cpp
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> res;
        stack<TreeNode*> st;
        TreeNode* curr = root;
        
        // Continue until we've processed all nodes
        while(curr || !st.empty()) {
            // Go as far left as possible
            while(curr) {
                st.push(curr);
                curr = curr->left;
            }
            
            // Process current node
            curr = st.top(); st.pop();
            res.push_back(curr->val);
            
            // Move to right subtree
            curr = curr->right;
        }
        return res;
    }
};
```

## 🔍 How It Works:
1. Start with the root node
2. Push all left children onto the stack until reaching a leaf
3. Pop the top node from stack and add its value to result
4. Move to the right subtree and repeat
5. Continue until stack is empty and current node is null

## ⏱️ Time & Space Complexity:
- Time Complexity: O(n) - We visit each node exactly once
- Space Complexity: O(n) - In worst case (skewed tree), stack can hold all nodes

## 📌 Key Takeaways:
- Stack-based traversal mimics recursion without using system call stack
- Always processes left subtree before root and right subtree
- For BST, produces elements in sorted order
- Can be modified for other traversal types (preorder, postorder)

## 🎯 Example:
```
     1
    / \
   2   3
  / \
 4   5
```
Inorder traversal result: [4, 2, 5, 1, 3]
