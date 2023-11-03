# 回溯算法理论

## 针对问题：

- 组合问题：从N个数里面按一定规则找出k个数的集合
- 切割问题：一个字符串按照一定规则有几种切割方式
- 子集问题：一个N个数的集合里有多少符合条件的子串
- 排列问题：N个数按一定规则全排列，有几种排列方式
- 棋盘问题：N皇后；解数独等

## 回溯法理解：

- 回溯法解决的问题都能够抽象为树形结构；
- 通过在集合中递归查找子集，集合的大小就构成了树的宽度，递归的深度就构成了树的深度
- 递归需要终止条件，因此必然是一颗高度有限的树（N叉树）

## 回溯法模板

```C++
void backtracking(arg){
    if (condition){
        result_save
            return;
    }
}
for (i in width)
```

- 遍历过程：在可能符合答案的集合当中搜索，集合大小构成树的宽度，递归深度构成树的深度
- for循环横向遍历集合区间，backtracking调用自己实现递归

![回溯算法理论基础](https://code-thinking-1253855093.file.myqcloud.com/pics/20210130173631174.png)

# [77. 组合](https://leetcode.cn/problems/combinations/)

## 题解思路

- 如果使用暴力循环的话需要k个数就要写k个循环，不具有普适性
- 使用回溯的思想就是到了问题的叶子结点就收集结果，然后将最后一个结果pop弹出进行回溯
- 递归（回溯）三部曲：
  - 确认递归函数的参数和返回值：startIndex作用是用于标记横向起始点
  - 确认终止条件（到达叶子结点）：path.size() == k
  - 单层递归逻辑：startIndex + 1 + push_back(i + 1)

- 剪枝优化：当起始位置后续的元素数量不足以满足k则可以被剪枝掉，即startIndex > n - k
  - 已经选择的元素个数path.size()
  - 还需要k - path.szie()
  - 在集合中至多要从该其实位置n - (k - path.size() ) + 1开始遍历（+1是因为需要左闭区间）
  - 其实可以将+1放到括号中去理解，筛选的是起始位置，其实是将其实位置左移一位，使得区间是个左闭区间

## 示例代码

```C++
class Solution {
public:
    vector<int> path;
    vector<vector<int>> result;
    void backtracking(int n, int k, int startIndex){
        if (path.size() == k) {
            result.push_back(path);
            return ;
        }
        for (int i = startIndex; i <= n; i++){
            path.push_back(i);
            backtracking(n, k, i + 1);
            path.pop_back();
        }
    }
    vector<vector<int>> combine(int n, int k) {
        result.clear();
        path.clear();
        backtracking(n, k, 1);
        return result;
    }
};
```

```C++
//剪枝版本
class Solution {
public:
    vector<int> path;
    vector<vector<int>> result;
    void backtracking(int n, int k, int startIndex){
        if (path.size() == k){
            result.push_back(path);
            return;
        }
        for (int i = startIndex; i <= n - (k - path.size()) + 1; i++){
            path.push_back(i);
            backtracking(n, k, i + 1);
            path.pop_back();
        }
    }
    vector<vector<int>> combine(int n, int k) {
        result.clear();
        path.clear();
        backtracking(n, k, 1);
        return result;
    }
};
```

# [216. 组合总和 III](https://leetcode.cn/problems/combination-sum-iii/description/)

## 题解思路

- 仿照上文组合的模板来写，给该题添加了几个参数
- 剪枝操作也和上文相同

## 示例代码

```C++
class Solution {
public:
    vector<int> path;
    vector<vector<int>> result;
    void backtracking(int k, int n, int sum, int startIndex){
        if (sum == n && path.size() == k){
            result.push_back(path);
            return;
        }
        for (int i = startIndex; i <= 9; i++){
            path.push_back(i);
            sum += i;
            backtracking(k, n, sum, i + 1);
            sum -= i;
            path.pop_back();
        }
    }
    vector<vector<int>> combinationSum3(int k, int n) {
        result.clear();
        path.clear();
        backtracking(k, n, 0, 1);
        return result;
    }
};
```

