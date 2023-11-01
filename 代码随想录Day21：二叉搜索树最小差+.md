

# [530. 二叉搜索树的最小绝对差 ](https://leetcode.cn/problems/minimum-absolute-difference-in-bst/)

## 题解思路

**二叉搜索树性质** ：使用中序遍历二叉搜索树的出来的结果是有序的

**1.递归法**

- 中序遍历二叉搜索树，将遍历顺序保存在一个容器中
- 在容器中进行差值比较，找最小的差值

**2.迭代法**

- 中序迭代遍历还是不够熟练，使用统一迭代法来记比较简单

## 示例代码

```C++
class Solution {
public:
    vector<int> vec;
    void traversal(TreeNode* node){
        if (node == NULL) return;
        if (node->left) traversal(node->left);
        vec.push_back(node->val);
        if (node->right) traversal(node->right);
    }
    int getMinimumDifference(TreeNode* root) {
        if (root == NULL) return 0;
        traversal(root);
        int minDiff = INT_MAX;
        for (int i = 1; i < vec.size(); i++){
            if (vec[i] - vec[i - 1] < minDiff) minDiff = vec[i] - vec[i - 1];
        }
        return minDiff;
    }
```

```C++
class Solution {
public:
    int getMinimumDifference(TreeNode* root) {
        vector<int> vec;
        stack<TreeNode*> st;
        int result = INT_MAX;
        TreeNode* cur = root;
        TreeNode* pre = NULL;
        while (cur != NULL || !st.empty()){
            if (cur != NULL){
                st.push(cur);
                cur = cur->left;
            }else {
                cur = st.top();
                st.pop();
                if (pre != NULL){
                    result = min(result, cur->val - pre->val);
                }
                pre = cur;
                cur = cur->right;
            }
        }
        return result;
    }
};
```

# [501. 二叉搜索树中的众数 ](https://leetcode.cn/problems/find-mode-in-binary-search-tree/)

## 题解思路

**1.递归法**

- 中序递归逻辑+众数判断：
  - 中序遍历保证了遍历是有序的
  - 当前后两个数值相同的时候，计数+1
  - 数值不同的时候计数回到1
  - 判断与maxCount的关系，大于则清空结果集，放入新元素，小于则舍弃

**2.迭代法**

- 迭代法的思路也是使用中序遍历+众数判断的逻辑

**3.常规二叉树**

- 如果不是二叉搜索树，直观方法就是使用一个map来统计频率，将频率排序，取出前面高频元素的集合
- 其中的难点为使用自定义的排序规则

## 示例代码

```C++
class Solution {
private:
    int maxCount = 0;
    int count = 0;
    TreeNode* pre = NULL;
    vector<int> result;
    void BSTsearch(TreeNode* cur){
        if (cur == NULL) return ;
        BSTsearch(cur->left);
        if (pre == NULL){
            count = 1;
        }else if (pre->val == cur->val) {
            count++;
        }else {
            count = 1;
        }
        pre = cur;
        if (count == maxCount){result.push_back(cur->val);}
        if (count > maxCount) {maxCount = count; result.clear(); result.push_back(cur->val);}
        BSTsearch(cur->right);
        return ;
    }
public:
    vector<int> findMode(TreeNode* root) {
        count = 0;
        maxCount = 0;
        pre = NULL;
        result.clear();
        BSTsearch(root);
        return result;
    }
};
```

```C++
class Solution {
public:
    vector<int> findMode(TreeNode* root) {
        vector<int> result;
        stack<TreeNode*> st;
        TreeNode* cur = root;
        TreeNode* pre = NULL;
        int count = 0;
        int maxCount = 0;

        while (!st.empty() || cur != NULL){
            if (cur != NULL){
                st.push(cur);
                cur = cur->left;
            } else{
                cur = st.top();
                st.pop();
                if (pre == NULL){
                    count = 1;
                }
                else if (pre->val == cur->val){
                    count++;
                }else {
                    count = 1;
                }
                if (count == maxCount){result.push_back(cur->val);}
                if (count > maxCount){
                    maxCount = count;
                    result.clear();
                    result.push_back(cur->val);
                }
                pre = cur;
                cur = cur->right;
            }
        }
        return result;
    }
};
```

```C++
class Solution {
private:
    void searchBST(TreeNode* cur, unordered_map<int, int>& map){
        if (cur == NULL) return;
        map[cur->val]++;
        searchBST(cur->left,map);
        searchBST(cur->right, map);
        return ;
    }
    bool static cmp(const pair<int, int>& a, const pair<int, int>& b){
        return a.second > b.second;
    }
public:
    vector<int> findMode(TreeNode* root) {
        unordered_map<int, int> map;
        vector<int> result;
        if (root == NULL) return result;
        searchBST(root, map);
        vector<pair<int, int>> vec(map.begin(), map.end());
        sort(vec.begin(), vec.end(), cmp);
        result.push_back(vec[0].first);
        for (int i = 1; i < vec.size(); i++){
            if (vec[i].second == vec[0].second) result.push_back(vec[i].first);
            else break;
        }
        return result;
    }
```

# [236. 二叉树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/)

## 题解思路

- 本题强调一个思想：搜索一条边和搜索一整棵树的写法类似但**不同**：
  - 搜索一条边：递归函数返回值不为空的时候，立刻返回
  - 搜索一整棵树：使用left和right接住返回值，根据后续处理逻辑回溯

```C++
//search for one edge
if (recursion_function(root->left)) return ;
if (recursion_function(root->right)) return;

//search for whole tree
left = recursuion_function(root->left);//left
right = recursion_function(root->right);//right
one level operation;//middle
```

- 代码解析：
  - 求最小公共祖先的方法是从底部向上遍历，只能是通过后序遍历的方式，实现自底向上的遍历
  - 左右子树的处理思路是遇到了目标值就往上返回，或者是遇到了空节点就往上返回，当一个结点的左右都有返回值的时候，这个节点就是最近的公共祖先
  - 处理二叉树是没法从下往上遍历的，但是处理逻辑是可以自下往上的处理
  - 在回溯过程中，必然要遍历整棵二叉树，即使已经找到结果，依然要把其他节点遍历完，上层递归函数才能得到下层的返回值

## 示例代码

```C++
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (root == q || root == p || root == NULL) return root;
        TreeNode* left = lowestCommonAncestor(root->left, p, q);
        TreeNode* right = lowestCommonAncestor(root->right, p, q);
        if (left != NULL && right != NULL) return root;
        if (left == NULL && right != NULL) return right;
        else if (left != NULL && right == NULL) return left;
        else {
            return NULL;
        }
    }
};
```

> 该题属于不好理解的题，需要经常复习和做类似的题来巩固

