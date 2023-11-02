# [669. 修剪二叉搜索树 ](https://leetcode.cn/problems/trim-a-binary-search-tree/)

## 题解思路

- 本题和直接删除节点的操作类似：通过将符合条件的结点子树指针直接给上层的结点来达到删除节点的目的
- 需要注意的是：删除超出右边界的大节点时，其左孩子可能是在边界内的；反之小节点右孩子也可能在边界内
- 递归代码总结：发现不符合边界的查找其左右孩子，将其符合边界的左右孩子返回给调用递归函数的上一层
- 迭代法的思路是先处理根结点，随后处理左右子树，先让根结点在范围内，然后对左右子树进行遍历，注意点和上面一样

## 示例代码

```C++
class Solution {
public:
    TreeNode* trimBST(TreeNode* root, int low, int high) {
        if (root == NULL) return nullptr;
        if (root->val < low){
            TreeNode* right = trimBST(root->right, low, high);
            return right;
        }
        if (root->val > high){
            TreeNode* left = trimBST(root->left, low, high);
            return left;
        }
        root->left = trimBST(root->left, low, high);
        root->right = trimBST(root->right, low, high);
        return root;
    }
};

```

```C++
class Solution {
public:
    TreeNode* trimBST(TreeNode* root, int low, int high) {
        if (root == NULL) return nullptr;

        while (root != NULL && (root->val < low || root->val > high)){
            if (root->val < low) root = root->right;
            else root = root->left;
        }// Root 现在在[low, high]之间了
        TreeNode* cur = root;
        while (cur != NULL){
            while (cur->left && cur->left->val < low)
            {
                cur->left = cur->left->right;
            }
            cur = cur->left;
        }
        
        cur = root;
        while (cur != NULL)
        {
            while (cur->right && cur->right->val > high){
                cur->right = cur->right->left;
            }
            cur = cur->right;
        }
        return root;
        
    }
};
```



# [108. 将有序数组转换为二叉搜索树](https://leetcode.cn/problems/convert-sorted-array-to-binary-search-tree/)

## 题解思路

- 递归终止条件是left>right，使用二分法构造子树
- 迭代法使用三个队列来模拟，一个队列放遍历节点，一个队列放在区间左区间下标，一个队列放右区间下标

## 示例代码

```C++
class Solution {
public:
    TreeNode* traversal(vector<int>& nums, int left, int right){
        if (left > right )return nullptr;
        int mid = left + ((right - left) / 2); 
        TreeNode* root = new TreeNode(nums[mid]);
        root->left = traversal(nums, left, mid - 1);
        root->right = traversal(nums, mid + 1, right);
        return root;
    }
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        return traversal(nums, 0, nums.size() - 1);
    }
};
```

```C++
class Solution {
public:
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        if (nums.size() == 0) return nullptr;

        TreeNode* root = new TreeNode(0);
        queue<TreeNode*> nodeQue;
        queue<int> leftQue;
        queue<int> rightQue;
        nodeQue.push(root);
        leftQue.push(0);
        rightQue.push(nums.size() - 1);

        while (!nodeQue.empty()){
            TreeNode* curNode = nodeQue.front();
            nodeQue.pop();
            int left = leftQue.front(); leftQue.pop();
            int right = rightQue.front(); rightQue.pop();
            int mid = left + ((right - left) / 2);

            curNode->val = nums[mid];

            if (left <= mid - 1){
                curNode->left = new TreeNode(0);
                nodeQue.push(curNode->left);
                leftQue.push(left);
                rightQue.push(mid - 1);
            }
            if (right >= mid + 1){
                curNode->right = new TreeNode(0);
                nodeQue.push(curNode->right);
                leftQue.push(mid + 1);
                rightQue.push(right);
            }
        }
        return root;
    }
};
```



# [538. 把二叉搜索树转换为累加树 ](https://leetcode.cn/problems/convert-bst-to-greater-tree/)

## 题解思路

- BST的中序有序提示应当按照反中序的顺序来叠加数值，其值就是从下往上累加的
- 递归：中节点的逻辑即加上前一个结点的值，用一个pre来更新累加值
- 迭代：使用中序模板来进行遍历

## 示例代码

```C++
class Solution {
private:
    int pre = 0;
    void traversal(TreeNode* node){
        if (node == NULL) return ;
        traversal(node->right);
        node->val += pre;
        pre = node->val;
        traversal(node->left);
    }
public:
    TreeNode* convertBST(TreeNode* root) { 
        traversal(root);
        return root;
    }
};
```

