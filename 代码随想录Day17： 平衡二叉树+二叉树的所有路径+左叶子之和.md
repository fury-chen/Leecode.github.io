# [110. 平衡二叉树 ](https://leetcode.cn/problems/balanced-binary-tree/)

## 题解思路

**1.递归法**

- 确定递归参数和返回值：getHeight的参数应当是当前遍历节点
- 确定终止条件：node==NULL 
- 确定单层递归逻辑：递归获取左右子树的高度，左右子树高度对比

**2.迭代法**

- 可以使用栈来模拟递归回溯的过程
- 当前结点不为空时就继续往下计算高度，当遇到空节点（叶子）的时候就回溯，向上高度-1

## 示例代码

```C++
class Solution {
public:
    int getHeight(TreeNode* node){
        if (node == NULL) return 0;
        int leftHeight = getHeight(node->left);
        if (leftHeight == -1) return -1;
        int rightHeight = getHeight(node->right);
        if (rightHeight == -1) return -1;

        int result;
        if (abs(leftHeight - rightHeight) > 1){result = -1;}
        else{result = 1 + max(leftHeight, rightHeight);}
        return result;
    }
    bool isBalanced(TreeNode* root) {
        return getHeight(root) == -1 ? false : true;
    }
};
```

```C++
class Solution {
public:
    int getHeight(TreeNode* cur){
        stack<TreeNode*> st;
        if (cur != NULL) st.push(cur);
        int depth = 0;
        int result = 0;
        while (!st.empty()){
            TreeNode* node = st.top();
            if (node != NULL){
                st.pop();
                st.push(cur);
                st.push(NULL);
                depth++;
                if (node->left) st.push(node->left);
                if (node->right) st.push(node->right);
            }else {
                st.pop();
                node = st.top();
                st.pop();
                depth--;
            }
            result = result > depth ? result : depth;
        }
        return result;
    }
    bool isBalanced(TreeNode* root) {
        stack<TreeNode*> st;
        if (root == NULL) return true;
        st.push(root);
        while (!st.empty()){
            TreeNode* node = st.top();
            st.pop();
            if (abs(getHeight(node->left) - getHeight(node->right)) > 1){
                return false;
            }
            if (node->right) st.push(node->right);
            if (node->left) st.push(node->left);
        }
        return true;
    }
};
```



# [257.二叉树的所有路径](https://leetcode.cn/problems/binary-tree-paths/)

## 题解思路

**1.递归法**

- 涉及回溯：需要回退路径来进入另一个路径
- 递归参数和返回值：记录路径的vector、存放结果的result、遍历的cur指针
- 递归终止条件：左右孩子都为空指针
- 单层循环：将路径记录下来，并将递归和回溯放在一起

**2.迭代法**

- 迭代法使用前序遍历来模拟遍历路径的过程
- 整体思路是遍历到叶子结点就将路径放入结果中，没到叶子结点就继续遍历左右节点

## 示例代码

```C++
class Solution {
public:
    void traversal(TreeNode* cur, vector<int>& path, vector<string>& result){
        path.push_back(cur->val);//中节点
        if (cur->left == NULL && cur->right == NULL){
            string sPath;
            for (int i = 0; i < path.size() - 1; i++){
                sPath += to_string(path[i]);
                sPath += "->";
            }
            sPath += to_string(path[path.size() - 1]);
            result.push_back(sPath);
            return ;
        }
        if (cur->left){
            traversal(cur->left, path, result);
            path.pop_back();
        }
        if (cur->right){
            traversal(cur->right, path, result);
            path.pop_back();
        }
    }
    vector<string> binaryTreePaths(TreeNode* root) {
        vector<string> result;
        vector<int> path;
        if (root == NULL) return result;
        traversal(root, path, result);
        return result;
    }
};
//compact version
class Solution {
public:
    void traversal(TreeNode* cur, string path, vector<string>& result){
        //定义递归函数的时候采用了复制赋值的方式，由此只传递值不使用引用，所以在递归返回上层的时候才有回溯
        path += to_string(cur->val);
        if (cur->left == NULL && cur->right == NULL){
            result.push_back(path);
            return;
        }
        if (cur->left) traversal(cur->left, path + "->", result);
        if (cur->right) traversal(cur->right, path + "->", result);
    }
    vector<string> binaryTreePaths(TreeNode* root) {    
        vector<string> result;
        string path;
        if (root == NULL) return result;
        traversal(root, path, result);
        return result;
    }
};
```

```C++
class Solution {
public:
    vector<string> binaryTreePaths(TreeNode* root) {
        stack<TreeNode*> treest;
        stack<string> pathStr;
        vector<string> result;
        if (root == NULL) return result;
        treest.push(root);
        pathStr.push(to_string(root->val));
        while (!treest.empty()){
            TreeNode* node = treest.top(); 
            treest.pop();
            string path = pathStr.top();pathStr.pop();
            if (node->left == NULL && node->right == NULL){
                result.push_back(path);
            }
            if (node->right){
                treest.push(node->right);
                pathStr.push(path + "->" + to_string(node->right->val));
            }
            if (node->left){
                treest.push(node->left);
                pathStr.push(path + "->" + to_string(node->left->val));
            }
        }
        return result;
    }
};
```



# [404. 左叶子之和 ](https://leetcode.cn/problems/sum-of-left-leaves/)

## 题解思路

**1.递归法**

- 对于左叶子结点的判断要通过父节点来判断
- 递归函数参数和返回值：当前结点
- 终止条件：遇到叶子结点
- 单层循环逻辑：判断叶子结点，如果是左叶子则加上
- 将整棵二叉树分为左右子树处理，递归子树

**2.迭代法**

- 对遍历顺序没有要求，使用前序要判断叶子结点并累加即可

## 示例代码

```C++
class Solution {
public:
    int sumOfLeftLeaves(TreeNode* root) {
        if (root == NULL) return 0;
        if (root->left == NULL && root->right == NULL) return 0;
        int leftValue = sumOfLeftLeaves(root->left);

        if (root->left && !root->left->left && !root->left->right){
            leftValue += root->left->val;
        }
        int rightValue = sumOfLeftLeaves(root->right);
        int sum = rightValue + leftValue;
        return sum;
    }
};
```

```C++
class Solution {
public:
    int sumOfLeftLeaves(TreeNode* root) {
        stack<TreeNode*> st;
        if (root != NULL) st.push(root);
        int sum = 0;
        while (!st.empty()){
            TreeNode* node = st.top();
            st.pop();
            if (node->left && !node->left->left && !node->left->right){
                sum += node->left->val;
            }
            if (node->left) st.push(node->left);
            if (node->right) st.push(node->right);
        }
        return sum;
    }
};
```



# 总结

- 通过平衡二叉树和二叉树最大最小深度了解到了求二叉树高度和深度的不同
  - 求二叉树的深度适合使用前序遍历（从根节点开始计算）
  - 求二叉树的高度适合使用后序遍历（从叶子结点开始递归）
- 二叉树路径的收集重点在于回溯的写法，其他的问题是操作的熟练度
