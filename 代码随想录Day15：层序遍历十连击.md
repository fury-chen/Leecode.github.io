# [102. 二叉树的层序遍历 ](https://leetcode.cn/problems/binary-tree-level-order-traversal/)

## 整体思路

**1.使用队列（迭代法）**

- 层序遍历要实现的是遍历完一层（当前结点），之后再遍历下一层（结点的左右子树）
- 需要一个队列来作为辅助控制遍历的顺序，通过先将当前层结点入队列，然后再将下一层的结点入队列，这样pop出来的顺序就是层序

## 示例代码

```C++
//iterate method
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> result;
        queue<TreeNode*> que;
        if (root != NULL) que.push(root);
        while (!que.empty()){
            int size = que.size();
            vector<int> vec;
            for (int i = 0; i < size; i++){
                TreeNode* node = que.front();
                que.pop();//pop之后就是push_back
                vec.push_back(node->val);
                if (node->left) que.push(node->left);
                if (node->right) que.push(node->right);
            }
            result.push_back(vec);
        }
        return result;
    }
};
```

```C++
//recursion method
class Solution {
public:
    void order(TreeNode* cur, vector<vector<int>>& result, int depth){
        if (cur == nullptr) return ;
        if (result.size() == depth) result.push_back(vector<int>());
        result[depth].push_back(cur->val);
        order(cur->left, result, depth + 1);
        order(cur->right, result, depth + 1);
    }
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> result;
        int depth = 0;
        order(root, result, depth);
        return result;
    }
};
```



# [107. 二叉树的层序遍历 II ](https://leetcode.cn/problems/binary-tree-level-order-traversal-ii/)

## 示例代码

```C++
class Solution {
public:
    vector<vector<int>> levelOrderBottom(TreeNode* root) {
        vector<vector<int>> result;
        queue<TreeNode*> que;
        if (root != NULL) que.push(root);
        while (!que.empty()){
            int size = que.size();
            vector<int> vec;
            for (int i = 0; i < size; i++){
                TreeNode* node = que.front();
                que.pop();
                vec.push_back(node->val);
                if (node->left) que.push(node->left);
                if (node->right) que.push(node->right);
            }
            result.push_back(vec);
        }
        reverse(result.begin(), result.end());
        return result;
    }
};
```



# [199. 二叉树的右视图](https://leetcode.cn/problems/binary-tree-right-side-view/)

## 示例代码

```C++
class Solution {
public:
    vector<int> rightSideView(TreeNode* root) {
        queue<TreeNode*> que;
        vector<int> result;
        if (root != NULL) que.push(root);
        while (!que.empty()){
            int size = que.size();

            for (int i = 0; i < size; i++){
                TreeNode* node = que.front();
                que.pop();
                if (i == (size - 1)) result.push_back(node->val);
                if (node->left) que.push(node->left);
                if (node->right) que.push(node->right);
            }
                 
        }
        return result;
    }
};
```

# [637. 二叉树的层平均值 ](https://leetcode.cn/problems/average-of-levels-in-binary-tree/)

## 示例代码

```C++
class Solution {
public:
    vector<double> averageOfLevels(TreeNode* root) {
        queue<TreeNode*> que;
        vector<double> result;
        if (root != NULL) que.push(root);
        while (!que.empty()){
            int size = que.size();
            double total = 0;
            for (int i = 0; i < size; i++){
                TreeNode* node = que.front();
                que.pop();
                total += (node->val);
                if (node->left) que.push(node->left);
                if (node->right) que.push(node->right);
            }
            result.push_back(total / size);
        }
        return result;
    }
};
```

# [429. N 叉树的层序遍历 ](https://leetcode.cn/problems/n-ary-tree-level-order-traversal/)

## 示例代码

- 需要注意到题目中给的children是vector

```C++
class Solution {
public:
    vector<vector<int>> levelOrder(Node* root) {
        queue<Node*> que;
        vector<vector<int>> result;
        if (root != NULL) que.push(root);
        while (!que.empty()){
            int size = que.size();
            vector<int> vec;
            for (int i = 0; i < size; i++){
                Node* node = que.front();
                que.pop();
                vec.push_back(node->val);
                for (int j = 0; j < node->children.size(); j++){
                    que.push(node->children[j]);
                }
            }
            result.push_back(vec);
        }
        return result;
    }
};
```

# [515. 在每个树行中找最大值](https://leetcode.cn/problems/find-largest-value-in-each-tree-row/)

## 示例代码

```C++
class Solution {
public:
    vector<int> largestValues(TreeNode* root) {
        queue<TreeNode*> que;
        vector<int> result;
        if (root != NULL) que.push(root);
        while (!que.empty()){
            int size = que.size();
            int max = INT32_MIN;
            for (int i = 0; i < size; i++){
                TreeNode* node = que.front();
                que.pop();
                if (node->val > max) max = node->val;
                if (node->left) que.push(node->left);
                if (node->right) que.push(node->right);
            }
            result.push_back(max);
        }
        return result;
    }
};
```

# [116. 填充每个节点的下一个右侧节点指针 ](https://leetcode.cn/problems/populating-next-right-pointers-in-each-node/)

## 示例代码

- Carl的思路和我的思路区别在于他是用前一个指针去指当前，将第一个指针特殊处理；而我是用当前指针指向后一个指针，对最后一个指针特殊处理，我的代码不够简洁

```C++
//My Version
class Solution {
public:
    Node* connect(Node* root) {
        queue<Node*> que;
        if (root != NULL) que.push(root);
        while (!que.empty()){
            int size = que.size();
            for (int i = 0; i < size; i++){
                if (i < size - 1){                
                    Node* cur = que.front();
                    que.pop();
                    Node* nextptr = que.front();
                    cur->next = nextptr;
                    if (cur->left) que.push(cur->left);
                    if (cur->right) que.push(cur->right);
                }else {
                    Node* cur = que.front();
                    que.pop();
                    cur->next = NULL;
                    if (cur->left) que.push(cur->left);
                    if (cur->right) que.push(cur->right);
                }

            }
        }
        return root;
    }
};
```

```C++
//Carl's Version
class Solution {
public:
    Node* connect(Node* root) {
        queue<Node*> que;
        if (root != NULL) que.push(root);
        while (!que.empty()) {
            int size = que.size();
            // vector<int> vec;
            Node* nodePre;
            Node* node;
            for (int i = 0; i < size; i++) {
                if (i == 0) {
                    nodePre = que.front(); // 取出一层的头结点
                    que.pop();
                    node = nodePre;
                } else {
                    node = que.front();
                    que.pop();
                    nodePre->next = node; // 本层前一个节点next指向本节点
                    nodePre = nodePre->next;
                }
                if (node->left) que.push(node->left);
                if (node->right) que.push(node->right);
            }
            nodePre->next = NULL; // 本层最后一个节点指向NULL
        }
        return root;

    }
};
```



# [104. 二叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/)

## 示例代码

```C++
class Solution {
public:
    int maxDepth(TreeNode* root) {
        queue<TreeNode*> que;
        int depth = 0;
        if (root != NULL) que.push(root);
        while (!que.empty()){
            int size = que.size();
            for (int i = 0; i < size; i++){
                TreeNode* node = que.front();
                que.pop();
                if (node->left) que.push(node->left);
                if (node->right) que.push(node->right);
            }
            depth++;
        }
        return depth;
    }
};
```

# [111. 二叉树的最小深度 ](https://leetcode.cn/problems/minimum-depth-of-binary-tree/)

## 示例代码

- 错误分析：想在循环当中返回depth 忘了满二叉树的情况；判断叶子结点的条件

```C++
class Solution {
public:
    int minDepth(TreeNode* root) {
        if (root == NULL) return 0;
        int depth = 0;
        queue<TreeNode*> que;
        que.push(root);
        while (!que.empty()){
            int size = que.size();
            depth++;
            for (int i = 0; i < size; i++){
                TreeNode* node = que.front();
                que.pop();
                if (node->left) que.push(node->left);
                if (node->right) que.push(node->right);
                if (!node->left && !node->right) {
                    return depth;
                }
            }
        }
        return depth;
    }
};
```

