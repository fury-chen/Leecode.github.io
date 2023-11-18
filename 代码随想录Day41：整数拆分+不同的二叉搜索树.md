# [343. 整数拆分](https://leetcode.cn/problems/integer-break/)

## 题解思路

**动态规划**

- dp数组含义：dp[i]表示拆分数字i，可以得到的最大乘积为dp[i]
- 确定递推公式：
  - 拆分成两个数：i*(i - j)
  - 拆分成多个数：j * dp[i - j]
- 初始化数组：dp[0] = 0 dp[1] = 0 dp[2] = 1
- 遍历顺序：从前往后，依赖i-j 的状态
- 举例：先拆成较小的数字，该数字的乘积最大值已经在前面被存起来了；为什么需要max(dp[i], max())——因为是从j=1开始尝试，需要更新找出最大值

**复杂度分析**

- 时间复杂度：O(n^2)
- 空间复杂度：O(n）

## 示例代码

```C++
class Solution {
public:
    int integerBreak(int n) {
        vector<int> dp(n + 1, 0);
        dp[2] = 1;
        for(int i = 3; i <= n; i++){//遍历dp
            for (int j = 1; j <= i; j++){
                dp[i] = max(max(j * (i - j), j * dp[i - j]), dp[i]);
            }
        }
        return dp[n];
    }
};
```

# [96. 不同的二叉搜索树 ](https://leetcode.cn/problems/unique-binary-search-trees/)

## 题解思路

**动态规划**

- dp数组含义：表示以i为头结点时的二叉搜索树的数量
- dp数组的推导公式：dp[i] = dp[i - 1] * dp[0] + dp[i - 2] * dp[1] ... -> dp[i] += dp[j - 1] * dp[i - j]
- dp数组的初始化：dp数组前面三个数都符合上述的递推，所以只需要初始化dp[0]即可
- 遍历顺序：从前往后
- 举例说明：

**复杂度分析**

- 时间复杂度：O(n^2）
- 空间复杂度：O(n)

## 示例代码

```C++
class Solution {
public:
    int numTrees(int n) {
        vector<int> dp(n + 1, 0);
        dp[0] = 1;
        for (int i = 1; i <= n; i++){
            for (int j = 1; j <= i; j++){
                dp[i] += dp[j - 1]* dp[i - j];
            }
        }
        return dp[n];
    }
};
```

