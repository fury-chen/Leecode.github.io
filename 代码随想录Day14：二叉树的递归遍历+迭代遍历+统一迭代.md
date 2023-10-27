# 二叉树的递归遍历

[94. 二叉树的中序遍历 - 力扣（LeetCode）](https://leetcode.cn/problems/binary-tree-inorder-traversal/)

[145. 二叉树的后序遍历 - 力扣（LeetCode）](https://leetcode.cn/problems/binary-tree-postorder-traversal/)

[144. 二叉树的前序遍历 - 力扣（LeetCode）](https://leetcode.cn/problems/binary-tree-preorder-traversal/)

## 题解思路

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
    void traversal(TreeNode* cur, vector<int>& vec){
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

# 二叉树的迭代遍历

## 题解思路

**1.栈的使用**

- 前序遍历时先加入右节点，后加入左节点；注意空节点不入栈
- 后序的时候想着顺序问题，以为能够颠倒一下就能使用，入栈顺序是中左右，出栈顺序是中右左，颠倒即可后序（出栈顺序决定遍历顺序）
- 中序遍历左中右，为了顺序要先将左子树的中全都入栈，先访问最左边的叶子结点，访问完之后中出栈，访问结点

**2.易错点**

- 后序遍历容易和前序遍历混淆
- 中序遍历的else当中不需要重新定义变量，只要用原本的cur即可

## 示例代码

```C++
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        stack<TreeNode*> st;
        vector<int> result;
        if (root == NULL) return result;
        st.push(root);
        while (!st.empty()){
            TreeNode* node = st.top();
            st.pop();
            result.push_back(node->val);
            if (node->right) st.push(node->right);
            if (node->left) st.push(node->left);
        }
        return result;
    }
};
```

```C++
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        stack<TreeNode*> st;
        vector<int> result;
        if (root == NULL) return result;
        st.push(root);
        while(!st.empty()){
            TreeNode* node = st.top();
            st.pop();
            result.push_back(node->val);
            if (node->left) st.push(node->left);
            if (node->right) st.push(node->right);
        }
        reverse(result.begin(), result.end());
        return result;
    }
};
```

```C++
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        stack<TreeNode*> st;
        vector<int> result;
        if (root == NULL) return result;
        TreeNode* cur = root;
        while (!st.empty() || cur != NULL){
            if (cur != NULL){
                st.push(cur);
                cur = cur->left;
            }else {
                cur = st.top();
                st.pop(); 
                result.push_back(cur->val);
                cur = cur->right;
            }
        }
        return result;
    }
};
```



# 二叉树的迭代统一遍历你

## 题解思路

**1.标记法**

- 为了解决访问结点和处理结点不一致的情况，将访问结点放入栈中，处理结点也放入栈中但做标记
- 标记方法：将访问的结点放入栈中后，紧接着放入一个空指针作为标记
- 中序遍历：概括思路就是按照右中左的顺序去遍历节点，但是没有直接处理中结点而是在中节点后面加上空节点，表示当遇到这个空节点的时候就处理=将其放入结果集
- 前序遍历：只需要改变处理指针的顺序即可；考察记忆
- 后序遍历：同上

## 示例代码

```c++
//inorder
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> result;
        stack<TreeNode*> st;
        if (root != nullptr) st.push(root);
        while (!st.empty()){
            TreeNode* node = st.top();
            if (node != NULL){
                st.pop();
                if (node->right) st.push(node->right);
                st.push(node);
                st.push(NULL);
                if (node->left) st.push(node->left);
            }else {
                st.pop();
                node = st.top();
                st.pop();
                result.push_back(node->val);
            }
        }
        return result;
    }
};
```

```C++
//preorder
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> result;
        stack<TreeNode*> st;
        if (root != NULL) st.push(root);
        while (!st.empty()){
            TreeNode* node = st.top();
                if (node != NULL){
                st.pop();
                if (node->right) st.push(node->right);
                if (node->left) st.push(node->left);
                st.push(node);
                st.push(NULL);
            }else {
                st.pop();
                node = st.top();
                st.pop();
                result.push_back(node->val);
            }

        
        }
        return result   ;
    }
};
```

```C++
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> result;
        stack<TreeNode*> st;
        if (root != NULL) st.push(root);
        while (!st.empty()){
            TreeNode* node = st.top();
            if (node != NULL) {
                st.pop();
                st.push(node);
                st.push(NULL);
                if (node->right) st.push(node->right);
                if (node->left) st.push(node->left);
            }else {
                st.pop();
                node = st.top();
                st.pop();
                result.push_back(node->val);
            }
        }
        return result;
        
    }
};
```

