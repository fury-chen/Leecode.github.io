# [235. 二叉搜索树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-search-tree/)

## 题解思路

- 递归思路和上一道最近公共祖先一样

## 示例代码

```C++
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (root == NULL) return root;
        if (root->val > p->val && root->val > q->val){
            TreeNode* left = lowestCommonAncestor(root->left, p, q);
            if (left != NULL)return left;
        }
        if (root->val < p->val && root->val < q->val){
            TreeNode* right = lowestCommonAncestor(root->right, p, q);
            if (right != NULL) return right;
        }
        return root;
    }
};
```

# [701. 二叉搜索树中的插入操作](https://leetcode.cn/problems/insert-into-a-binary-search-tree/)

## 题解思路

- 简单理解：只需要找到空节点插入就可以了
- 复杂理解：按照有序性去遍历，找到叶子结点来插入（没差倒是）
- 递归思路：将插入的子树当做结点插入到原本位置
- 如何使用递归函数返回值完成新加入结点父子关系赋值操作，下一层将加入节点返回，本层使用root->right或者root->left接住
- 如果不使用返回值，则找到插入结点位置，直接让其父节点指向插入结点，结束递归即可

## 示例代码

```C++
TreeNode* insertIntoBST(TreeNode* root, int val) {
        if (root == NULL) {
            TreeNode* node = new TreeNode(val);
            return node;
        }
        if (root->val > val) root->left = insertIntoBST(root->left, val);
        if (root->val < val) root->right = insertIntoBST(root->right, val);
        return root;
    }
};
//without return 
class Solution {
public:
    TreeNode* parent;
    void traversal(TreeNode* cur, int val){
        if (cur == NULL) {
            TreeNode* node = new TreeNode(val);
            if (val > parent->val) parent->right = node;
            else {parent->left = node;}
            return ;
        }
        parent = cur ; 
        if (cur->val > val) traversal(cur->left, val);
        if (cur->val < val) traversal(cur->right, val);
        return;
    }
    TreeNode* insertIntoBST(TreeNode* root, int val) {
        parent = new TreeNode(0);
        if (root == NULL){root = new TreeNode(val);}
        traversal(root, val);
        return root;
    }
};
```

```C++
class Solution {
public:
    TreeNode* insertIntoBST(TreeNode* root, int val) {
        if (root == NULL) {
            TreeNode* node = new TreeNode(val);
            return node;
        }
        TreeNode* cur = root;
        TreeNode* parent = root;
        while (cur != NULL){
            parent = cur;
            if (cur->val > val) cur = cur->left;
            else cur = cur->right;
        }
        TreeNode* node = new TreeNode(val);
        if (val < parent->val) parent->left = node;
        else parent->right = node;
        return root;
    }
};
```



# [450. 删除二叉搜索树中的节点](https://leetcode.cn/problems/delete-node-in-a-bst/)

## 题解思路

**1.递归法**

- 分为五种情况：
  - 没有找到删除节点，遍历到空节点直接返回
  - 找到删除节点：
    - 左右孩子都为空，直接删除结点，返回NULL作为根结点
    - 删除结点的左孩子为空，右孩子不为空，删除结点，右孩子不为，返回右孩子根结点；
    - 和上一种相反，左孩子不为空，同理，即返回下层值给上一层，当前值就被删除了（但要记得释放内存）
    - 左右孩子都不为空，找该结点右子树的最左结点，将左子树移到这个节点上，将右子树返回给上层
- 终止条件：
  - 找到删除对应的点=将其子树返回给上层
  - 遍历到空节点，返回空节点个上层结点去接住
- 小结：删除和插入结点都是返回修改过的下层结点给上层结点接住

## 示例代码

```C++
class Solution {
public:
    TreeNode* deleteNode(TreeNode* root, int key) {
        if (root == NULL) return root;
        if (root->val == key) {
            if (root->left == nullptr && root->right == nullptr){
                delete root;
                return nullptr;
            }
            else if (root->left == nullptr){
                auto retNode = root->right;
                delete root;
                return retNode;
            }
            else if (root->right == nullptr){
                auto retNode = root->left;
                delete root;
                return retNode;
            }
            else {
                TreeNode* cur = root->right;
                while (cur->left != nullptr){
                    cur = cur->left;
                }
                cur->left = root->left;
                TreeNode* tmp = root;
                root = root->right;
                delete tmp;
                return root;
            }
        }
        if (root->val > key) root->left = deleteNode(root->left, key);
        if (root->val < key) root->right = deleteNode(root->right, key);
        return root;
    }
};
```

