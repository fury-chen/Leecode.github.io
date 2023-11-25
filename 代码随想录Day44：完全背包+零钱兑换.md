# 完全背包问题

- 对比01背包

```C++
for (int i = 0; i < weight.size(); i++){
    for (int j = bagWeight; j >= weight[i]; j--){
        dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);
    }
}
```

```C++
for (int i = 0; i < weight.size(); i++){
    for (int j = weight[i]; j <= bagWeight; j++){
        dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);
    }
}
```

- 注意事项：

  - 遍历顺序可以颠倒，可以先遍历背包后遍历物品

  - 先物品再背包是横向更新数值，先背包再物品是纵向去更新数值，二者都可以由前面的数值更新出来的；即横向纵向取最大值来更新

    

  

# [携带研究材料（第七期模拟笔试](https://kamacoder.com/problempage.php?pid=1052)

```C++
#include<iostream>
#include<vector>
using namespace std;
 
int main(){
    int N, V;
    cin >> N >> V;
    vector<int> researchWeight ;
    vector<int> researchValue ;
    for (int i = 0; i < N; i++){
        int w, v;
        cin >> w >> v;
        researchValue.push_back(v);
        researchWeight.push_back(w);
        //cout << researchWeight[i];
         
    }
    std::vector<int> dp(V + 1, 0);
    for (int i = 0; i < N; i++){
        for (int j = 0; j <= V; j++){
            if (j - researchWeight[i] >= 0)
                dp[j] = max(dp[j], dp[j - researchWeight[i]] + researchValue[i]);
            //cout << dp[i] << endl;
        }
    }
    cout << dp[V] << endl;
    return 0;
}
```

# [518. 零钱兑换 II ](https://leetcode.cn/problems/coin-change-ii/)

## 题解思路

**完全背包**

- dp数组的含义：dp数组表示组合成j的组合数
- 递推公式：dp[j] += dp[j - coins[i]]
- 初始化：dp[0] = 1因为是累加，所以起码有一种方法
- 遍历顺序：
  - 遍历物品->遍历背包：按照coins的顺序去遍历（组合数）
  - 遍历背包->遍历物品：排列数
- 打印dp数组

**复杂度分析**

- 时间复杂度：O(mn）
- 空间复杂度：O(m)

## 示例代码

```C++
class Solution {
public:
    int change(int amount, vector<int>& coins) {
        vector<int> dp(50001, 0);
        dp[0] = 1;
        for (int i = 0; i < coins.size(); i++){
            for (int j = coins[i]; j <= amount; j++){
                dp[j] += dp[j - coins[i]]; 
            }
        }
        return dp[amount];
    }
};
```

# [377. 组合总和 Ⅳ](https://leetcode.cn/problems/combination-sum-iv/)

## 题解思路

- 题意当中有误导含义：说是求组合，但是本质上是求符合条件的排列，如果要求的是排列的所有结果，就需要用到回溯算法，而题目要求的是求排列和，即符合要求的排列有多少种

**完全背包**

- dp数组含义：符合目标的排列个数
- 确定递推公式：装满背包的公式为dp[i] += dp[i - nums[j]]
- 初始化：当背包为0的时候认为为0种可能
- 确定遍历顺序：
  - 求组合：外层for循环物品，内层for循环为背包
  - 求排列：外层背包，内层物品

**复杂度分析**

- 时间复杂度：O(target*n)
- 空间复杂度：O(target)

## 示例代码

```C++
class Solution {
public:
    int combinationSum4(vector<int>& nums, int target) {
        vector<int> dp(target + 1, 0);
        dp[0] = 1;
        for (int i = 0; i <= target; i++){
            for (int j = 0; j < nums.size(); j++){
                if (i - nums[j] >= 0 && dp[i] < INT_MAX - dp[i - nums[j]]){
                    dp[i] += dp[i - nums[j]];
                }
            }
        }
        return dp[target];
    }
};
```

