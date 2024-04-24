# ACM二叉树的遍历

- 本题的重要点在于如何构造二叉树，并且二叉树的构造

```C++
#include <iostream>
#include <vector>
#include <unordered_map>
using namespace std;

struct TreeNode{
    char val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(char val) : val(val), left(nullptr), right(nullptr){}
};

TreeNode* buildTree(unordered_map<char, pair<char, char>>& nodeMap, char rootValue){
    if (rootValue == '0'){
        return nullptr;
    }
    
    TreeNode* root = new TreeNode(rootValue);
    int leftChild = nodeMap[rootValue].first;
    int rightChild = nodeMap[rootValue].second;
    
    root->left = buildTree(nodeMap, leftChild);
    root->right = buildTree(nodeMap, rightChild);
    
    return root;
}

void preorderTraversal(TreeNode* root){
    if (root == nullptr) return ;
    cout << root->val;
    preorderTraversal(root->left);
    preorderTraversal(root->right);
}

void postorderTraversal(TreeNode* root){
    if (root == nullptr) return ;
    postorderTraversal(root->left);
    postorderTraversal(root->right);
    cout << root->val;
}

void inorderTraversal(TreeNode* root){
    if (root == nullptr) return ;
    inorderTraversal(root->left);
    cout << root->val;
    inorderTraversal(root->right);
}

int main(){
    int n;
    cin >> n;
    unordered_map<char, std::pair<char, char>> nodeMap;
    
    vector<char> index(n + 1, '0');
    vector<vector<int>> nums(n + 1, vector<int>(2, 0));
    for (int i = 1; i <= n; i++){
        cin >> index[i] >> nums[i][0] >> nums[i][1];
    }
    
    for (int i = 1; i <= n; i++){
        nodeMap[index[i]] = make_pair(index[nums[i][0]], index[nums[i][1]]);
    }
    TreeNode* root = buildTree(nodeMap, index[1]);
    
    preorderTraversal(root);
    cout << endl;
    
    inorderTraversal(root);
    cout << endl;
    
    postorderTraversal(root);
    cout << endl;
    
    return 0;
}
```

