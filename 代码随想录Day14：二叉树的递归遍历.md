# 144. 二叉树的递归遍历

## 题解思路

[94. 二叉树的中序遍历 - 力扣（LeetCode）](https://leetcode.cn/problems/binary-tree-inorder-traversal/)

[145. 二叉树的后序遍历 - 力扣（LeetCode）](https://leetcode.cn/problems/binary-tree-postorder-traversal/)

[144. 二叉树的前序遍历 - 力扣（LeetCode）](https://leetcode.cn/problems/binary-tree-preorder-traversal/)

**1.递归三部曲**

- 确定递归函数的参数和返回值
- 确定终止条件
- 确定单层递归逻辑

**2.易错点**

- 在写traversal函数的时候，vec变量总是忘了加引用，导致存到不同的位置
- 忘记写终止条件

## 示例代码

```C++
class Solution{
public:
    void traversal(TreeNode* cur, vector<int> vec){
        if (cur == NULL) return ;
        vec.push_back(cur->val);
        traversal(cur->left, vec);
        traversal(cur->right, vec);
    }
    vector<int> preorderTraversal(TreeNode* root){
        vector<int> result;
        traversal(root, result);
        return result;
    }
};
```

```C++
class Solution {
public:
    void traversal(TreeNode* cur, vector<int>& vec){
        if (cur == NULL) return;
        traversal(cur->left, vec);
        traversal(cur->right, vec);
        vec.push_back(cur->val);
    }
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> result;
        traversal(root, result);
        return result;
    }
};
```

```C++
class Solution {

public:
    void traversal(TreeNode* cur, vector<int>& vec){
        if (cur == NULL) return;
        traversal(cur->left, vec);
        vec.push_back(cur->val);
        traversal(cur->right, vec);
    }
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> result;
        traversal(root, result);
        return result;
    }
};
```

