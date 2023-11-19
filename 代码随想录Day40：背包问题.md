# 背包问题理论基础

## 01背包问题（二维数组）

- 问题描述：只有n件物品和最多能背重量为w的背包，第i件物品的重量是weight[i]，得到的价值是value[o]。每件物品只能用一次，求解将哪些背包装入背包里物品的价值总和最大

- 分析方法：每件物品只有两种状态，取或者不取，所以可以使用回溯来搜索所有的情况，其复杂度为$O(n^2)$，如果使用暴力解法就是指数级的复杂度

- 动态规划解法优化：

  <img src="https://code-thinking-1253855093.file.myqcloud.com/pics/20210110103003361.png" alt="动态规划-背包问题1" style="zoom:50%;" />

  - dp下标及其含义：使用而额外数组dp[i] [j]表示从下标[0 - i]的物品里任意取，放进容量为j的背包，价值总和最大的是多少
  - 确定递推公式：有两个方向来推导出dp[i] [j]	
    - 不放物品i：由dp[i - 1] [j]推出，即不放物品i的最大价值，当前物品不放进背包的价值（和前一个状态相同）
    - 放入物品i：由dp[i - 1] [j -weight]推出，dp[i - 1] [j - weight[i]] 为背包容量j - weight[i]的时候不放物品i的最大价值
    - 推出递推公式dp[i] [j] = max(dp[i - 1] [j], dp[i - 1] [j - weight[i] ] + value[i])
  - 如何初始化：
    - 首先从定义出发如果j = 0则价值总和肯定为0
    - 从推导公式出发发现i的状态是由i - 1推导出来的，则i = 0的时候一定要初始化
      - 如果j<weight[0]则说明第一个物品是放不进去的
      - 当j >= weight[0]的时候，dp[0] [j]应该是value[0]，因为背包容量足够放编号0物品
      - 总结一下就是背包容量不够的时候就不放
  - 遍历顺序：两个for循环遍历物品和容量
    - 先遍历物品，然后遍历背包重量
    - 先遍历背白，再遍历物品

```C++
for (int j = 0; j < weight[0]; j++){
    dp[0][j] = 0;
}
for (int j = weight[0]; j <= bagweight; j++){
    dp[0][j] = value[0];
}
```

- 抓住核心：
  - 物品状态只有0 1 两种状态，背包也只有装得下和装不下两种状态——01背包问题就是通过对放物品和不放物品i的讨论来进行循环，对二维数组进行遍历
  - 先试着放物品1，然后在放物品1 的基础上放物品2 的价值，如果容量不够则继承上方的dp值
  - 难点在于讨论初始化，除了第一排第一列之外其他的都是要讨论的
  - 明确dp数组的含义为：在j容量下，放第i个物品的价值
  - for循环可以颠倒数据

## [01背包例题](https://kamacoder.com/problempage.php?pid=1046)

```c++
#include<bits/stdc++.h>
using namespace std;

int n, bagweight;
void solve(){
    std::vector<int> weight(n, 0) ;
    std::vector<int> value(n, 0) ;
    for (int i = 0; i < n; ++i){
        cin >> weight[i];
    }
    for (int j = 0; j < n; ++j){
        cin >> value[j];
    }
    vector<vector<int>> dp(weight.size(), vector<int>(bagweight + 1, 0));
    
    //inilialize the dp array
    for (int j = weight[0]; j <= bagweight; j++){
        dp[0][j] = value[0];
        //first colunm put the first obj into it
    }
    for (int i = 1; i < weight.size(); i++){
        for (int j = 0; j <= bagweight; j++){
            if (j <  weight[i]) dp[i][j] = dp[i - 1][j];
            else {
                dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - weight[i]] + value[i]);
            }
        }
    }
    std::cout << dp[weight.size() - 1][bagweight] << std::endl;
    
}
int main(){
    while(cin >> n >> bagweight){
        solve();
    }
    return 0;
}
```

## 01背包问题（一维数组）

- 一维数组的可行性：背包是由上层和左上数据确定的，因此可以将上层数据直接进行拷贝到下一层数组，形成滚动数组的计算

- 递推公式变化：dp[j] = max (dp[j], dp[j - weight[i]] + value[i])
- 遍历顺序：for遍历物品+for遍历背包（倒序）

```C++
#include<iostream>
#include<vector>
using namespace std;

int main(){
    int researchType, suitcaseSpace;
    cin >> researchType >> suitcaseSpace;
    
    vector<int> materialSize(researchType);
    vector<int> researchValue(researchType);
    
    for (int i = 0; i < researchType; i++){
        cin >> materialSize[i];
    }
    for (int j = 0; j < researchType; j++){
        cin >> researchValue[j];
    }
    vector<int> dp(suitcaseSpace + 1, 0);
    
    for (int i = 0; i < researchType; i++){
        for (int j = suitcaseSpace; j >= materialSize[i]; j--){
            dp[j] = max(dp[j], dp[j - materialSize[i]] + researchValue[i]);
        }
    }
    std::cout << dp[suitcaseSpace] << std::endl;

    return 0;
}
```

