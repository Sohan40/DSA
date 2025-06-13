# 🧾 Problem: [Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal)

## 🧠 Brute Force:
- Use recursion to traverse the tree.

## 💡 Optimal Approach:
- Use an explicit stack for iterative inorder traversal (no call stack usage).

## 💻 Code:
```cpp
vector<int> inorderTraversal(TreeNode* root) {
    vector<int> res;
    stack<TreeNode*> st;
    TreeNode* curr = root;
    while(curr || !st.empty()) {
        while(curr) {
            st.push(curr);
            curr = curr->left;
        }
        curr = st.top(); st.pop();
        res.push_back(curr->val);
        curr = curr->right;
    }
    return res;
}
```

## ⏱️ Time & Space Complexity:
- Time: O(n)
- Space: O(n)

## 📌 Key Takeaways:
- Stack-based traversal mimics recursion.
