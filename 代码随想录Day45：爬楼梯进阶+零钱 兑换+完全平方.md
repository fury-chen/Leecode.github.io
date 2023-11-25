# [爬楼梯进阶](https://kamacoder.com/problempage.php?pid=1067)

## 题解思路

**完全背包**

- 本题为装满m阶背包的组合数，注意是从第一级台阶开始计数，循环也要从第一级台阶开始
- dp含义：爬到i个台阶有dp[i]种方法
- 递推公式：完全背包组合dp[i] = dp[i - nums[j]]
- 确定遍历顺序：将target放在循环外层，将nums放在循环内层
- 举例推导：
- 个人理解：dp的含义是爬到第i个台阶的方法，而递推公式表示的意思是当前的状态是由前面的j层可能性推导出来的（横纵两个方向进行推导）

## 示例代码

```C++
#include<iostream>
#include<vector>
using namespace std;
int main(){
    int n, m;
    while (cin >> n >> m){
        std::vector<int> dp(n + 1, 0);
        dp[0] = 1;
        for (int i = 1; i <= n; i++){
            for (int j = 1; j <= m; j++){
                if (i - j >= 0)
                    dp[i] += dp[i - j];
            }
        }
        std::cout << dp[n] << std::endl;
    }    
    
}

```

# [322. 零钱兑换 ](https://leetcode.cn/problems/coin-change/)

## 题解思路

**完全背包**

- dp数组含义：凑足总额为j的钱币最少需要多少个coins
- 递推公式：dp = min(dp[j - coins[i]], dp[j])
- 初始化数组：凑足0个金币的初值为0
- 遍历顺序：钱币有无顺序对最小个数没有影响（组合数——先物品后背包）
- 举例推导：

**复杂度分析**

- 时间复杂度：O(n*amount)，n为coins的长度
- 空间复杂度：O(amount)

## 示例代码

```C++
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        vector<int> dp(amount + 1, INT_MAX);
        dp[0] = 0;
        for (int i = 0; i < coins.size(); i++){
            for (int j = coins[i]; j <= amount; j++){
                if (dp[j - coins[i]] != INT_MAX)
                    dp[j] = min(dp[j - coins[i]] + 1, dp[j]);
            }
        }
        if (dp[amount] == INT_MAX) return -1;
        return dp[amount];
    }
};
```

# [279. 完全平方数](https://leetcode.cn/problems/perfect-squares/)

## 题解思路

**完全背包**

- dp[i]： 凑成目标正整数为i的排列个数
- 递推公式：min(dp[j - i * i] + 1, dp[j])
- dp数组初始化：dp[0] = 1
- 确定遍历顺序：组合数外层遍历物品，内层for遍历背包；排列数外层遍历背包，内层物品
- dp数组推导

## 示例代码

```C++
class Solution {
public:
    int numSquares(int n) {
        vector<int> dp(n + 1, INT_MAX);
        dp[0] = 0;
        for (int i = 0; i <= n; i++){
            for (int j = 1; j * j <= i; j++){
                dp[i] = min(dp[i], dp[i - j * j] + 1);
            }
        }
        return dp[n];
    }
}; 
```

