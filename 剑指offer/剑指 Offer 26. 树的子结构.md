# [LCR 143. 子结构判断 ](https://leetcode.cn/problems/shu-de-zi-jie-gou-lcof/description/)

## 题解思路

- 讨论：B树必须完全匹配，因此需要有一个检查当前是否匹配的逻辑和一个递归检查的逻辑
- 注意第一个判断是A || B 其一为空的情况
- A的根结点和B的根结点讨论（递归讨论左右子树节点）
  - 相同：递归比较其左右子节点
  - 不相同：讨论A的左子树和B的根结点；讨论A的右子树和B的根结点
- 主函数是讨论根结点及其左右节点；isSub是讨论任一节点往下的情况

## 示例代码

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool isSubStructure(TreeNode* A, TreeNode* B) {
        if (A == NULL || B == NULL){
            return false;
        }//1、空树不算任何树的子结构
        if (A->val == B->val && isSub(A->left, B->left) && isSub(A->right, B->right)){
            return true;
        }//2、当根结点相同
        return (isSubStructure(A->left, B) || isSubStructure(A->right, B));
        //3、讨论根结点不同的情况：A左孩子和B根结点， A右孩子和B根结点
    }

    bool isSub(TreeNode* A,  TreeNode* B){
        if (A == NULL && B == NULL){
            return true;
        }
        if (B == NULL && A != NULL){
            return true;
        }
        if (B != NULL && A == NULL){
            return false;
        }
        //讨论两者都为空和其一为空的情况
        //讨论都不为空时的情况
        if (A->val == B->val){
            return (isSub(A->left, B->left) && isSub(A->right, B->right));
        } else {
            return false;
        }
    }
};
```

