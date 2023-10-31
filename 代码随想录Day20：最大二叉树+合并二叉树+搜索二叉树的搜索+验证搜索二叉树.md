 

# [700. 二叉搜索树中的搜索 ](https://leetcode.cn/problems/search-in-a-binary-search-tree/description/)

## 题解思路

**1.递归法**

- 按照二分搜索的思路进行前序搜索
- 使用变量保存节点

**2.迭代法**

- 根据二叉搜索树的有序性，遍历的过程中不需要回溯

## 实例代码

```c++
class Solution {
public:
    TreeNode* searchBST(TreeNode* root, int val) {
        if (root == NULL || root->val == val) return root;
        TreeNode* result = NULL;
        if (root->val > val) result = searchBST(root->left, val);
        if (root->val < val) result = searchBST(root->right, val);
        return result;
    }
};
```

```c++
class Solution {
public:
    TreeNode* searchBST(TreeNode* root, int val) {
        while (root != NULL) {
            if (root->val > val) root = root->left;
            else if (root->val < val) root = root->right;
            else return root;
        }
        return NULL;
    }
};
```

# [98. 验证二叉搜索树 ](https://leetcode.cn/problems/validate-binary-search-tree/description/)

## 题解思路

**1.递归法**

- 递归函数的参数和返回值：没有需要保存的路径，因此不需要返回值
- 递归函数的终止条件：中序遍历到叶子结点
- 递归函数的单层逻辑：中序遍历将二叉树的值都保存到一个容器中
- 检查容器是否升序，升序则说明是二叉搜索树

**2.迭代法**

- 中序遍历保存数值到容器中，升序判断

## 示例代码

```c++
class Solution {
	public:
    vector<int> vec;
    void traversal(TreeNode* root){
        if (root == NULL) return ;
        traversal(root->left);
        vec.push_back(root->val);
        traversal(root->right);
    }
    bool isValidBST(TreeNode* root) {
        vec.clear();
        traversal(root);
        for (int i = 1; i < vec.size(); i++){
            if (vec[i - 1] >= vec[i]) return false;
        }
        return true;
    }
};
```

```c++
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        stack<TreeNode*> st;
        TreeNode* cur = root;
        TreeNode* pre = NULL;
        while (cur != NULL || !st.empty()){
            if (cur != NULL){
                st.push(cur);
                cur = cur->left;
            }
            else {
                cur = st.top();
                st.pop();
                if (pre != NULL && cur->val <= pre->val){
                    return false;
                }
                pre = cur;
                cur = cur->right;
            }
        }
        return true;
    }
};
```

