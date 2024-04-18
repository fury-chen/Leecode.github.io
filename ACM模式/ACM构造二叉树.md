# 构造二叉树
- https://kamacoder.com/problempage.php?pid=1020
```C++
#include <iostream>
#include <string>
using namespace std;

struct TreeNode {
    char val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(char _val)  : val(_val), left(nullptr), right(nullptr) {}
};
//总结此处的几个关键数据：
//中序遍历结果中的中节点index,前序遍历开始和结束index
TreeNode* traversal(string& preorder, string& inorder){
    if (preorder.length() == 0) return nullptr;
    char rootValue = preorder[0];
    TreeNode* root = new TreeNode(rootValue);
    // cout << rootValue << endl;
    
    if (preorder.size() == 1) return root;
    int index = 0;
    while (inorder[index] != rootValue){
        index++;
    }
    // cout << "index = " << index << endl;
    string leftInorder = inorder.substr(0, index);
    string rightInorder = inorder.substr(index + 1, inorder.size() - 1);
    //cout << "leftsize : " << leftInorder.size() << endl;
    //cout << "rightsize :" << rightInorder.size() << endl;
    // cout << leftInorder << endl;
    // cout << rightInorder << endl;
    
    string leftPreorder = preorder.substr(1, leftInorder.size());
    // cout << leftPreorder <<endl;
    string rightPreorder = preorder.substr(index + 1 , rightInorder.size());
    // cout << rightPreorder << endl;
    root->left = traversal(leftPreorder, leftInorder);
    root->right = traversal(rightPreorder, rightInorder);
    return root;
}

void postorderTraversal(TreeNode* root) {
    if (root == nullptr) {
        return;
    }

    postorderTraversal(root->left);
    postorderTraversal(root->right);
    cout << root->val;
}


int main(){
    string s;
    while (getline(cin, s)){
        //多组测试数据
        string preorder = "", inorder = "";
        int i;
        for (i = 0; s[i] != ' '; i++) preorder += s[i];
        i++;
        for (; i < s.size(); i++) inorder += s[i];
        // cout << preorder << endl;
        // cout << inorder << endl;
        TreeNode* root = traversal(preorder, inorder);
        postorderTraversal(root);
        cout << endl;
    }
    return 0;
}
```
