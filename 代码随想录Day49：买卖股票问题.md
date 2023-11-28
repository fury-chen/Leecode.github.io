# [121. 买卖股票的最佳时机 ](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock/)

## 题解思路

**贪心算法**

- 因为只买卖一次，只要找到左最小值和右最大值就行

**动态规划**

- dp数组含义：dp[i] [0]表示第i天持有股票所得最多现金，即不论何时买卖，手中的现金资产是唯一衡量标准，表示了出售之后的状态也表示了买入的状态
- 如果第i天持有股票为dp[i] [0]，那么可以由两个状态推出
  - 第i - 1天就持有股票，则今天保持现状，今日所得就是昨日持有股票所得现金，即dp[i - 1] [0]
  - 第i天买入股票，所得现金就是买入今天买入的现金，即：-prices[i]（持有股票现金流是负数，的max表示成本越少）
  - 所以dp[i] [0] = max(dp[i - 1] [0], -prices[i])
- 如果今天不持有股票为dp[i] [1]，则可由两个状态推导出来
  - 第i - 1天就不持有股票，那么就保持现状，所得现金就是昨天不持有股票所得现金，即：dp[i - 1] [1]
  - 第i天就卖出股票，所得现金就是按照今天股票价格卖出后所得现金price[i] + dp[i - 1] [0]

- dp数组初始化：基础数组都是要从dp[0] [0] 和dp[0] [1]推导出来的
- 确定遍历顺序：从前往后
- 优化：dp[i]只依赖dp[i - 1]因此可以将其优化为滚动数组

**复杂度分析**

- 贪心
  - 时间复杂度：O(n)
  - 空间复杂度：O(1)
- 动规
  - 时间复杂度：O(n)
  - 空间复杂度：O(n)

## 示例代码

```C++
//贪心算法
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int low = INT_MAX;
        int result = 0;
        for (int i = 0; i < prices.size(); i++){
            low = min(low, prices[i]);
            result = max(result, prices[i] - low);
        }
        return result;
    }
};
//DP
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int len = prices.size();
        if (len == 0) return 0;
        vector<vector<int>> dp(len, vector<int>(2));
        dp[0][0] -= prices[0];
        dp[0][1] = 0;
        for (int i = 1; i < len; i++){
            dp[i][0] = max(dp[i - 1][0], -prices[i]);
            dp[i][1] = max(dp[i - 1][1], prices[i] + dp[i - 1][0]);
        }
        return dp[len - 1][1];
    }
};
//optimize version
```

# [122. 买卖股票的最佳时机 II](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/)

## 题解思路

**动态规划**

- 与前一题思路一样，区别在于对多次买卖的处理
  - 第i - 1天就不持有股票，保持现状，所得现金就是昨天不持有股票的先进所的dp[i - 1] [1]
  - 第i天卖出股票，所得现金就是按照今天股票价格卖出后得到的现金price[i] + dp[i  - 1] [0]

## 示例代码

```C++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int len = prices.size();
        vector<vector<int>> dp(len, vector<int>(2, 0));
        dp[0][0] = -prices[0];
        dp[0][1] = 0;        
        for (int i = 1; i < len; i++){
            dp[i][0] = max(dp[i - 1][0], dp[i - 1][1] - prices[i]);//注意这里的区别，多次买卖需要加上前一个手上现金流
            dp[i][1] = max(dp[i - 1][1], dp[i - 1][0] + prices[i]);
        }
        return dp[len - 1][1];
    }
};
```

# [123. 买卖股票的最佳时机 III ](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iii/)

## 题解思路

**动态规划**

- dp数组含义：一天总共五个状态（0.没有操作 1.第一次持有股票 2.第一次不持有股票  3.第二次持有股票 4. 第二次不持有股票）；dp[i] [j]表示第i天的j个状态
- 确定递推公式：要达到dp[i] [1]有两种操作（第i天买入和沿用前一天的持有状态），dp[i] [2]也同理，因此每个状态都是延续或发生变化
  - dp[i] [1] = max(dp[i - 1] [0] - price[i], dp[i - 1] [1])
  - dp[i] [2] = max(dp[i - 1] [1] + price, dp[i - 1] [2])
  - .....（根据是卖出还是买入决定）
- dp数组如何初始化：第0天没有操作因此dp[0] [0] = 0 dp[0] [2] = 0，后续的卖出也都没有操作dp[0] [2] = 0 ，dp[0] [4] = 0；而第二次买入操作dp[0] [3] = -price[0] 
- 遍历顺序：从前往后

**复杂度分析**

- 时间复杂度 ：O(n)
- 空间复杂度：O(n*5)

## 示例代码

```C++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if (prices.size() == 0) return 0;
        vector<vector<int>> dp(prices.size(), vector<int>(5, 0));
        dp[0][1] = -prices[0];
        dp[0][3] = -prices[0];
        for (int i = 1; i < prices.size(); i++){
            dp[i][0] = dp[i - 1][0];
            dp[i][1] = max(dp[i - 1][1], dp[i - 1][0] - prices[i]);
            dp[i][2] = max(dp[i - 1][2], dp[i - 1][1] + prices[i]);
            dp[i][3] = max(dp[i - 1][3], dp[i - 1][2] - prices[i]);
            dp[i][4] = max(dp[i - 1][4], dp[i - 1][3] + prices[i]);
        }
        return dp[prices.size() - 1][4];
    }
};
```

# [188. 买卖股票的最佳时机 IV ](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iv/)

## 题解思路

- 根据上述思路：奇数次状态买入，偶数次专题卖出 ，因此状态总数为 2 * k + 1
- 初始化时，所有技术位置都要初始化为-price[0]

## 示例代码

```C++
class Solution {
public:
    int maxProfit(int k, vector<int>& prices) {
        if (prices.size() == 0) return 0;
        vector<vector<int>> dp(prices.size(), vector<int>(2 * k + 1, 0));
        for (int j = 1; j < 2 * k; j += 2){
            dp[0][j] = - prices[0];
        }
        for (int i = 1; i < prices.size(); i++){
            for (int j = 0; j < 2 * k - 1; j += 2){
                dp[i][j + 1] = max(dp[i - 1][j + 1], dp[i - 1][j] - prices[i]);
                dp[i][j + 2] = max(dp[i - 1][j + 2], dp[i - 1][j + 1] + prices[i]);
            }
        }
        return dp[prices.size() - 1][2 * k];
    }
};
```

