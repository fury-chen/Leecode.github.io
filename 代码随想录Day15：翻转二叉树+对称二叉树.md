

# [226.翻转二叉树](https://leetcode.cn/problems/invert-binary-tree/)

## 题解思路

**1.递归法**

- 确定递归函数参数和返回值：传入结点指针
- 确定终止条件：当结点为空的时候返回
- 确定单层递归逻辑：交换两个节点
- 先孩子或者先父母都行，而孩子父母孩子顺序不行，因为孩子会被翻转两次

**2.迭代法**

- 使用栈来进行前序遍历，当访问中节点之后进行翻转
- 只需要在前序遍历的代码基础上加上一段交换节点代码

## 示例代码

```C++
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if (root == NULL) return root;
        swap(root->left, root->right);
        invertTree(root->left);
        invertTree(root->right);
        return root;
    }
};
```

```C++
lass Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if (root == NULL) return root;
        stack<TreeNode*> st;
        st.push(root);
        while (!st.empty()){
            TreeNode* node = st.top();
            st.pop();
            swap(node->left, node->right);
            if (node->left) st.push(node->left);
            if (node->right) st.push(node->right);
        }
        return root;
    }
};
```

# [101. 对称二叉树 ](https://leetcode.cn/problems/symmetric-tree/)

## 题解思路

**1.比较子树方法（递归）**

- 比较的对象是子树的里侧和外侧的元素；
- 遍历顺序只能是后序遍历，因为是要遍历两棵树并且一棵树是左右中，一棵树是右左中；
- 递归三部曲：
  - 确定递归函数的参数和返回类型：两棵子树，bool类型比较这两棵子树的值是否相同
  - 确定终止条件：
    - 左右节点一个为空，不对称，FALSE
    - 左右节点都为空，对称，TRUE
    - 左右节点不为空，而数值不同，FALSE
  - 单层递归逻辑：
    - 比较二叉树外侧是否对称：传入左节点的左孩子，右节点的右孩子
    - 比较内侧是否对称，传入左节点的右孩子和右节点的左孩子
    - 内外相同返回TRUE，不同返回FALSE

**2.比较子树方法（迭代）**

- 使用迭代法的受要注意，迭代法不再涉及三个次序的迭代写法，本质上与遍历方式无关，可以使用队列来比较两棵树是否互相翻转（匹配）——队列或栈擅长
- 使用遍历时传入同一层的内外侧结点，进行两两比较

**3.总结**

- 这道题用两种视角去看就是不同的问题：
- 递归——后序遍历比较两棵子树的左右节点是否相同
- 迭代——子树结点的匹配问题

## 示例代码

```C++
class Solution {
public:
    bool compare(TreeNode* left, TreeNode* right){
        if (left == NULL && right != NULL) return false;
        else if (left != NULL && right == NULL) return false;
        else if (left == NULL && right == NULL) return true;
        else if (left->val != right->val) return false;
        
        bool outside = compare(left->left, right->right);
        bool inside = compare(left->right, right->left);
        bool isSame = outside && inside;
        return isSame;
    }
    bool isSymmetric(TreeNode* root) {
        if (root == NULL )return true;
        return compare(root->left, root->right);
    }   
};
```

```C++\
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        if (root == NULL) return true;
        queue<TreeNode*> que;
        que.push(root->left);
        que.push(root->right);
        while (!que.empty()){
            TreeNode* leftNode = que.front(); que.pop();
            TreeNode* rightNode = que.front(); que.pop();
            if (!leftNode && !rightNode) {
                continue;//left and righ are both empty
            }
            if (!leftNode || !rightNode || (leftNode->val != rightNode->val)){
                return false;
            }
            que.push(leftNode->left);
            que.push(rightNode->right);
            que.push(rightNode->left);
            que.push(leftNode->right);
        }
        return true;
    }
};
```

