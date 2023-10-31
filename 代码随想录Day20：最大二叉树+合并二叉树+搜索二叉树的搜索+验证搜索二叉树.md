# [654. 最大二叉树 ](https://leetcode.cn/problems/maximum-binary-tree/)

## 题解思路

**1.递归法**

- 确认递归函数参数和返回值：本题不需要记录特殊条件路径，不需要返回值，并且改进上可以直接在原数组操作下标，所以参数为数组及其左右下标
- 确认终止条件：左下标和右下标相等
- 确认单层递归逻辑：递归构造根结点的左右子树

## 示例代码

```C++
class Solution {
public:
    TreeNode* traversal(vector<int>& nums, int left, int right){
        if (left >= right){return NULL;}

        int maxValueIndex = left;
        for (int i = left + 1; i < right; i++){
            if (nums[i] > nums[maxValueIndex])  maxValueIndex = i;
        }
        TreeNode* root = new TreeNode(nums[maxValueIndex]);
        root->left = traversal(nums, left, maxValueIndex);
        root->right = traversal(nums, maxValueIndex + 1, right);
        return root;
    }
    TreeNode* constructMaximumBinaryTree(vector<int>& nums) {
        return traversal(nums, 0, nums.size());
    }
};
```

# [617. 合并二叉树 -](https://leetcode.cn/problems/merge-two-binary-trees/)

## 题解思路

**1.递归法**

- 递归函数的参数和返回值：两个二叉树的根结点（及其子树）
- 终止条件：当两个节点都为空时
- 单层递归逻辑：
  - 两个节点其一为空，返回另一个结点
  - 两个结点都不为空时，将2+到1上
  - 递归左右子树

**2.迭代法**

- 处理两个树，使用队列进行层序遍历，只需要判断root1左右为空和不为空的四种情况

## 示例代码

```C++
class Solution {
public:
    TreeNode* mergeTrees(TreeNode* root1, TreeNode* root2) {
        if (root1 == NULL) return root2;
        if (root2 == NULL) return root1;
        queue<TreeNode*> que;
        que.push(root1);
        que.push(root2);
        while (!que.empty()){
            TreeNode* node1 = que.front(); que.pop();
            TreeNode* node2 = que.front(); que.pop();
            node1->val += node2->val;
            if (node1->left != NULL && node2->left != NULL){
                que.push(node1->left);
                que.push(node2->left);
            }
            if (node1->right != NULL && node2->right != NULL){
                que.push(node1->right);
                que.push(node2->right);
            }
            if (node1->left == NULL && node2->left != NULL){
                node1->left = node2->left;
            }
            if (node1->right == NULL && node2->right != NULL){
                node1->right = node2->right;
            }
        }
        return root1;
    }
};
```

```C++
class Solution {
public:
    TreeNode* mergeTrees(TreeNode* t1, TreeNode* t2) {
        if (t1 == NULL) return t2;
        if (t2 == NULL) return t1;
        // 重新定义新的节点，不修改原有两个树的结构
        TreeNode* root = new TreeNode(0);
        root->val = t1->val + t2->val;
        root->left = mergeTrees(t1->left, t2->left);
        root->right = mergeTrees(t1->right, t2->right);
        return root;
    }
};
```

 

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

