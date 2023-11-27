# [198. 打家劫舍 ](https://leetcode.cn/problems/house-robber/)

## 题解思路

**动态规划**

- dp数组下标含义：下标为i（包括i）的可偷窃金币
- 递推公式：决定于前一个房间的状态 和 前前一个房间与当前房间的和dp[i] = max(dp[i - 1], dp[i - 2] + nums[i])
- 初始化：初始化01位置的情况，0时只有一个房间，为了最大化肯定要，1时根据前一个房间和当前房间的最大值决定
- 遍历顺序：从小到大
- 举例说明

**复杂度分析**

- 时间复杂度：O(n)
- 空间复杂度：O(n)

## 示例代码

```C++
class Solution {
public:
    int rob(vector<int>& nums) {
        if (nums.size() == 0) return 0;
        if (nums.size() == 1) return nums[0];
        vector<int> dp(nums.size());
        dp[0] = nums[0];
        dp[1] = max(nums[0], nums[1]);
        for (int i = 2; i < nums.size(); i++){
            dp[i] = max(dp[i - 2] + nums[i], dp[i - 1]);
            cout << dp[i] << endl;
        }
        return dp[nums.size() - 1];
    }

};
```

# [213. 打家劫舍 II ](https://leetcode.cn/problems/house-robber-ii/)

## 题解思路

**DP**

- 三种情况讨论：
  - 将首尾元素隔离，考虑中间线性部分
  - 将首元素作为线性部分考虑
  - 将尾元素作为线性部分考虑
- 和打家劫舍I一样的逻辑，只是将环形转变为了线性

## 示例代码

```C++
class Solution {
public:
    int rob(vector<int>& nums) {
        if (nums.size() == 0) return 0;
        if (nums.size() == 1) return nums[0];
        int result1 = robRange(nums, 0, nums.size() - 2);
        int result2 = robRange(nums, 1, nums.size() - 1);
        return max(result1, result2);
    }
    int robRange(vector<int>& nums, int start, int end){
        if (end == start) return nums[start];
        vector<int> dp(nums.size());
        dp[start] = nums[start];
        dp[start + 1] = max(nums[start + 1], nums[start]);
        for (int i = start + 2; i <= end; i++){
            dp[i] = max(dp[i - 2] + nums[i], dp[i - 1]);
        }
        return dp[end];
    }
};
```

# [337. 打家劫舍 III](https://leetcode.cn/problems/house-robber-iii/)



## 题解思路

**树形状态转移**

- 递归参数和返回值：使用dp数组记录当前状态偷与不偷，使用一个长度为2的数组就能够保存当前结点的状态（递归系统栈中会保存每一层递归的参数）
- 确定终止条件：当遇到空节点时不论偷与不偷都对结果没有影响，因此返回
- 确定遍历顺序：后序遍历
- 单层递归逻辑：偷当前结点，左右孩子就不偷，反之偷左右孩子

**复杂度分析**

- 时间复杂度：O(n)，每个节点遍历一次
- 空间复杂度：O(logn)，系统递推栈的空间

## 示例代码

```C++
class Solution {
public:
    int rob(TreeNode* root) {
        vector<int> result = robTree(root);
        return max(result[0], result[1]);
    }
    vector<int> robTree(TreeNode* cur){
        if (cur == NULL) return vector<int>{0, 0};
        vector<int> left = robTree(cur->left);
        vector<int> right = robTree(cur->right);
        int val1 = cur->val + left[0] + right[0];
        int val2 = max(left[0], left[1]) + max(right[0], right[1]);
        return {val2, val1};
    }
};
```

