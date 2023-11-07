# [面试题 17.12. BiNode ](https://leetcode.cn/problems/binode-lcci/)

## 题解思路

- 使用中序遍历能够升序遍历BST的结点，对每个节点的操作即将cur->right指向当前结点，并更新当前cur 指针为node
- 记住：遍历到哪里就将cur->right指向这个节点，只要遍历顺序是中序即可，随后更新cur指针
- 使用一个虚拟头结点，其右孩子指向最左下角的结点，递归直到最左下角的时候才开始执行这个操作

## 示例代码

```C++
class Solution {
private:
    TreeNode* head = new TreeNode(0);
    TreeNode* cur = head;
    void traversal(TreeNode* node){
        if (node == NULL) return;
        if (node->left) traversal(node->left);
        node->left = NULL;
        cur->right = node;
        cur = node;
        if (node->right) traversal(node->right);
    }
public:
    TreeNode* convertBiNode(TreeNode* root) {
        traversal(root);
        return head->right;
    }
};
```

