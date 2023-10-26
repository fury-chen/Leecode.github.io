# [104. 二叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/)

## 题解思路

**1.基本概念**

- 二叉树结点的深度：从根结点到该结点的最长简单路径边的条数或者节点数（取决于深度从0开始还是从1开始）
- 二叉树结点的高度：从该结点到叶子结点的最长简单路径边的条数或者节点数（取决于0 or 1）
- 根结点的高度 = 二叉树的最大深度

**2.递归法**

- 确定递归参数和返回值：参数为传入树的根结点，返回类型为int
- 确定终止条件：如果为空节点就返回0
- 确定单层递归的逻辑：求左子树的深度和右子树的深度，取其最大值+1（算上当前的中间节点；后序）

**3.迭代法**

- 通过层序遍历，每遍历一层结束就在depth上++
- 层序遍历模板题

## 示例代码

```C++
//postorder recursion
class Solution {
public:
    int getDepth(TreeNode* node){
        if (node == NULL) return 0;
        int leftDepth = getDepth(node->left);
        int rightDepth = getDepth(node->right);
        int depth = 1 + max(leftDepth, rightDepth);
        return depth;
    }
    int maxDepth(TreeNode* root) {
        return getDepth(root);
    }
};
//compact version
class solution {
public:
    int maxDepth(TreeNode* root) {
        if (root == null) return 0;
        return 1 + max(maxDepth(root->left), maxDepth(root->right));
    }
};
// preorder recursion
class Solution {
public:
    int result;
    void getDepth(TreeNode* node, int depth){
        result = depth > result ? depth : result;
        if (node->left == NULL && node->right == NULL) return ;
        
        if (node->left){
            depth++;
            getDepth(node->left, depth);
            depth--;
        }
        if (node->right){
            depth++;
            getDepth(node->right, depth);
            depth--;
        }//回溯算法的思路，试探有就深度+1，当回到上一层结点就-1
        return;
    }
    int maxDepth(TreeNode* root) {
        result = 0;
        if (root == NULL) return result;
        getDepth(root, 1);
        return result;
    }
};
```

```C++
class Solution {
public:
    int maxDepth(TreeNode* root) {
        queue<TreeNode*> que;
        if (root != NULL) que.push(root);
        int depth = 0;
        while (!que.empty()){
            int size = que.size();
            for (int i = 0; i < size; i++){
                TreeNode* node = que.front();
                que.pop();
                if (node->left) que.push(node->left);
                if (node->right) que.push(node->right);
            }
            depth++;
        }
        return depth;
    }
};
```



# [559. N 叉树的最大深度 ](https://leetcode.cn/problems/maximum-depth-of-n-ary-tree/)

## 示例代码

**1.迭代法***

```C++
class Solution {
public:
    int maxDepth(Node* root) {
        queue<Node*> que;
        if (root != NULL) que.push(root);
        int depth = 0;
        while (!que.empty()){
            int size = que.size();
            for (int i = 0; i < size; i++){
                Node* node = que.front();
                que.pop();
                for (int j = 0; j < node->children.size(); j++){
                    que.push(node->children[j]);
                }
            }
            depth++;
        }
        return depth;
    }
};
```

**2.递归法**

```C++
class Solution {
public:
    int maxDepth(Node* root) {
        if (root == 0) return 0;
        int depth = 0;
        for (int i = 0; i < root->children.size(); i++){
            depth = max(depth, maxDepth(root->children[i]));
        }
        return depth + 1;

    }
};
```

# [111. 二叉树的最小深度 ](https://leetcode.cn/problems/minimum-depth-of-binary-tree/)

## 题解思路

**1.递归法**

- 确定递归参数和返回值：int的深度
- 确定终止条件：遇到空节点的时候返回
- 确定单层递归逻辑：
  - 左子树为空而右子树不为空的情况会被最大深度的逻辑漏掉
  - 左子树为空而右子树不为空则最小深度是1+右子树深度
  - 如果左右子树都不为空，则最小深度是左右子树的最小值+1

**2.迭代法**

- 依然是使用层序遍历的模板
- 只有左右孩子都为空的时候才是到达最低点（叶子结点判断）

## 示例代码

```C++
class Solution {
public:
    int getDepth(TreeNode* node){
        if (node == NULL) return 0;
        int leftDepth = getDepth(node->left);
        int rightDepth = getDepth(node->right);
        if (node->left == NULL && node->right != NULL){
            return 1 + rightDepth;
        } 
        if (node->left != NULL && node->right == NULL){
            return 1 + leftDepth;
        }
        int result = 1 + min(leftDepth, rightDepth);
        return result;
    }
    int minDepth(TreeNode* root) {
        return getDepth(root);
    }
};
```

```C++
class Solution {
public:

    int minDepth(TreeNode* root) {
        if (root == NULL) return 0;
        int depth = 0;
        queue<TreeNode*> que;
        que.push(root);
        while(!que.empty()) {
            int size = que.size();
            depth++; // 记录最小深度
            for (int i = 0; i < size; i++) {
                TreeNode* node = que.front();
                que.pop();
                if (node->left) que.push(node->left);
                if (node->right) que.push(node->right);
                if (!node->left && !node->right) { 
                    // 当左右孩子都为空的时候，说明是最低点的一层了，退出
                    return depth;
                }
            }
        }
        return depth;
    }
};
```

# [222. 完全二叉树的节点个数](https://leetcode.cn/problems/count-complete-tree-nodes/)

## 题解思路

**1.递归法**

- 递归三部曲：将二叉树节点分为左右两个子树递归，终止条件是空节点
- 时间复杂度：O(n)
- 空间复杂度：O(logn)

**2.迭代法**

- 层序遍历法统计个数
- 时间复杂度：O(n)
- 空间复杂度：O(n)

## 示例代码

```C++
class Solution {
public:
    int getNodeNum(TreeNode* cur){
        if (cur == NULL) return 0;
        int leftNum = getNodeNum(cur->left);
        int rightNum = getNodeNum(cur->right);
        return leftNum + rightNum + 1;
    }
    int countNodes(TreeNode* root) {
        return getNodeNum(root);
    }
};
```



```C++
class Solution {
public:
    int countNodes(TreeNode* root) {
        if (root == NULL) return 0;
        queue<TreeNode*> que;
        int result = 0;
        que.push(root);
        while (!que.empty()){
            int size = que.size();
            for (int i = 0; i < size; i++){
                TreeNode* node = que.front();
                que.pop();
                result++;
                if (node->left) que.push(node->left);
                if (node->right) que.push(node->right);
            }
        }
        return result;
    }
};
```

# 小结

```
今天这部分的内容看到了二叉树的基本属性的求解方式，如果不涉及二叉树的某些性质，关于树本身的高度、深度、节点数都可以用层序逐个遍历出来
如果使用递归的话就要按照递归三部曲的思考方式
注意最大最小深度之间的逻辑区别
```

