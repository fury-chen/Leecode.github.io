# [513. 找树左下角的值](https://leetcode.cn/problems/find-bottom-left-tree-value/)

## 题解思路

**1.递归法**

- 本题需要两个全局变量来判断是否找到最左结点：最大深度+result
- 确定递归参数和返回值：遍历根结点+深度变量
- 确定终止条件：遇到叶子结点，更新最大深度
- 单层递归逻辑：回溯控制深度
- 递归的深度求解基本都要涉及回溯，一开始不知道深度最大的方向，只能是试探性的深度搜索，最后还要回退

**2.迭代法**

- 迭代法很容易想到层序遍历，每次遍历一层的时候记录第一个元素，输出最后一层第一个元素就是最左边的元素

## 示例代码

```C++
class Solution {
public:
    int maxDepth = INT_MIN;
    int result;
    void traversal(TreeNode* root, int depth){
        if (!root->left && !root->right){
            if (depth > maxDepth){
                maxDepth = depth;
                result = root->val;
            }
            return ;
        }
        if (root->left){
            traversal(root->left, depth + 1);
        }
        if (root->right){
            traversal(root->right, depth + 1);
        }
        return;
    }
    int findBottomLeftValue(TreeNode* root) {
        traversal(root, 0);
        return result;
    }
};
```



```C++
class Solution {
public:
    int findBottomLeftValue(TreeNode* root) {
        queue<TreeNode*> que;
        int leftValue;
        if (root != NULL) que.push(root);
        while (!que.empty()){
            int size = que.size();
            for (int i = 0; i < size; i++){
                TreeNode* node = que.front();
                que.pop();
                if (i == 0) leftValue = node->val;
                if (node->left) que.push(node->left);
                if (node->right) que.push(node->right);
            }
            
            
        }
        return leftValue;
    }
};
```

# [112. 路径总和 ](https://leetcode.cn/problems/path-sum/)

## 题解思路

**1.递归法**

- 确定递归函数和返回值：
  - 需要搜索整棵二叉树不用处理递归返回值，递归函数就不需要返回值
  - 如果需要搜索和处理，就需要返回值
  - 如果搜索其中一个符合条件的路径，需要返回值
- 终止条件中有两点：到达叶子结点，和不符合；到达叶子结点，和符合
- 单层递归循环：回溯思路，递归左右子树，带上计数器
- 注意终止条件的优先级：强条件（子集）放在前面

**2.迭代法**

- 使用栈做模拟回溯，栈不仅要记录结点指针，还要记录从头结点到此节点的路径总值和（使用pair来存放）
- 除了操作上使用了pair方法之外其他的逻辑于前序遍历思路类似

## 示例代码

```C++
class Solution {
public:
    bool traversal(TreeNode* cur, int count){
        if (!cur->left && !cur->right && count == 0) return true;
        if (!cur->left && !cur->right) return false;
        
        if (cur->left){
            if (traversal(cur->left, count - cur->left->val)) return true;
        }
        if (cur->right){
            if (traversal(cur->right, count - cur->right->val) )return true;
        }
        return false;
    }
    bool hasPathSum(TreeNode* root, int targetSum) {
        if (root == NULL) return false;
        return traversal(root, targetSum - root->val);
    }
};
```

```C++
class Solution {
public:
    bool hasPathSum(TreeNode* root, int targetSum) {
        if (root == NULL) return false;
        stack<pair<TreeNode*, int>> st;
        st.push(pair<TreeNode*, int>(root, root->val));
        while (!st.empty()){
            pair<TreeNode*, int> node = st.top();
            st.pop();
            if (!node.first->left && !node.first->right && targetSum == node.second) return true;

            if (node.first->right){
                st.push(pair<TreeNode*, int> (node.first->right, node.second + node.first->right->val));
            }
            if (node.first->left){
                st.push(pair<TreeNode*, int> (node.first->left, node.second + node.first->left->val));
            }
        }
        return false;
    }
};
```

# [106. 从中序与后序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

## 题解思路

- 第一步是先找到根结点——后序的最后一个元素
- 第二步是切割中序数组，将其分为左右两个子树
- 第三步是将中序数组的两个子树大小与后序的子树大小匹配
- 第四步是递归，将四个分出来的子树进行递归

## 示例代码

```C++
class Solution {
public:
    TreeNode* traversal(vector<int>& inorder, vector<int>& postorder){
        if (postorder.size() == 0) return NULL;
        TreeNode* root = new TreeNode(postorder[postorder.size() - 1]);

        if (postorder.size() == 1) return root;
        int detemiterIndex ;
        for (detemiterIndex = 0; detemiterIndex < inorder.size(); detemiterIndex++){
            if (inorder[detemiterIndex] == postorder[postorder.size() - 1]) break;
        }

        vector<int> leftInorder(inorder.begin(), inorder.begin() + detemiterIndex);
        vector<int> rightInorder(inorder.begin() + detemiterIndex + 1, inorder.end());

        postorder.resize(postorder.size() - 1);
        vector<int> leftPostorder(postorder.begin(), postorder.begin() + leftInorder.size());
        vector<int> rightPostorder(postorder.begin() + leftInorder.size(), postorder.end());

        root->left = traversal(leftInorder, leftPostorder);
        root->right = traversal(rightInorder, rightPostorder);
        return root;
    }
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        if (inorder.size() == 0 || postorder.size() == 0) return NULL;
        return traversal(inorder, postorder);
    }
};
```

```C++
//https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-inorder-traversal/
class Solution {
public:
    TreeNode* traversal(vector<int>& inorder, vector<int>& preorder){
        if (preorder.size() == 0) return NULL;
        int rootValue = preorder[0];
        TreeNode* root = new TreeNode(rootValue);
        if (preorder.size() == 1) return root;

        int delimiterIndex ;
        for (delimiterIndex = 0; delimiterIndex < inorder.size(); delimiterIndex++){
            if (inorder[delimiterIndex] == rootValue) break;
        }

        vector<int> leftInorder(inorder.begin(), inorder.begin() + delimiterIndex);
        vector<int> rightInorder(inorder.begin() + delimiterIndex + 1, inorder.end());

        vector<int> leftPreorder(preorder.begin() + 1, preorder.begin() + 1 + leftInorder.size());
        vector<int> rightPreorder(preorder.begin() + 1 + leftInorder.size(), preorder.end());
        
        root->left = traversal(leftInorder, leftPreorder);
        root->right = traversal(rightInorder, rightPreorder);
        
        return root;
    }
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        if (preorder.size() == 0 || inorder.size() == 0) return NULL;
        return traversal(inorder, preorder);
    }
};
```

