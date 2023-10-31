# 二叉搜索树最小差
## 示例代码
```C++
lass Solution {
public:
    vector<int> vec;
    void traversal(TreeNode* node){
        if (node == NULL) return;
        if (node->left) traversal(node->left);
        vec.push_back(node->val);
        if (node->right) traversal(node->right);
    }
    int getMinimumDifference(TreeNode* root) {
        if (root == NULL) return NULL;
        vec.clear();
        traversal(root);
        int minValue = INT_MAX;
        for (int i = 1; i < vec.size(); i++){
            if (vec[i] - vec[i - 1] < minValue) minValue = vec[i] - vec[i - 1];
        }
        return minValue;
    }
};
```
