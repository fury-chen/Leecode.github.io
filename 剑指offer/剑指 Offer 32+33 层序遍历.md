

# [剑指 Offer 32 - I. 从上到下打印二叉树](https://www.acwing.com/problem/content/41/)

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
    vector<int> printFromTopToBottom(TreeNode* root) {
        vector<int> result;
        queue<TreeNode*> que;
        if (root != NULL) que.push(root);
        while (!que.empty()){
            int size = que.size();
            for (int i = 0; i < size; i++){
                TreeNode* cur = que.front();
                que.pop();
                if (cur->left) que.push(cur->left);
                if (cur->right) que.push(cur->right);
                result.push_back(cur->val);
            }
        }
        return result;
    }
};
```



# [剑指 Offer 32 - II. 从上到下打印二叉树 II](https://www.acwing.com/problem/content/42/)

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
    vector<vector<int>> printFromTopToBottom(TreeNode* root) {
        vector<int> layerVec;
        vector<vector<int>> result;
        queue<TreeNode*> que;
        if (root != NULL) que.push(root);
        while (!que.empty()){
            int size = que.size();
            layerVec.clear();
            for (int i = 0; i < size; i++){
                TreeNode* cur = que.front();
                que.pop();
                layerVec.push_back(cur->val);
                if (cur->left) que.push(cur->left);
                if (cur->right) que.push(cur->right);
            }
            result.push_back(layerVec);
        }
        return result;
    }
};
```

- 在层序遍历的基础上使用一个path来保存每层的结果，每次遍历层的时候将其放入结果集中
