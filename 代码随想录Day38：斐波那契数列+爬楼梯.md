# [509. 斐波那契数 ](https://leetcode.cn/problems/fibonacci-number/)

## 题解思路

**动规五步**

- 确定dp[i] 的含义：第i个斐波那契数值
- 确定递推公式：dp[i] = dp[i - 1] + dp[i - 2]
- dp数组如何初始化：按照dp[0] = 1, dp[1] = 1
- 确定遍历顺序：从前向后遍历
- 打印dp数组

**复杂度分析**

- 时间复杂度：O(n)
- 空间复杂度：O(1)

## 示例代码

```C++
class Solution {
public:
    int fib(int n) {
        if (n <= 1) return n;
        vector<int> dp(n + 1);
        dp[0] = 0;
        dp[1] = 1;
        for (int i = 2; i <= n; i++){
            dp[i] = dp[i - 1] + dp[i - 2];
        }
        return dp[n];
    }
};
//只需要维护两个元素
int a = 0; int b = 1;
for (int i = 2; i <= n; i++){
 	tmp = b;
    b += a;
    a = tmp;
}
```

```C++
//recursion
class Solution {
public:
    int fib(int n) {
        if ( n < 2) return n;
        return fib(n - 1) + fib(n - 2);
    }
};
```

# [70. 爬楼梯 ](https://leetcode.cn/problems/climbing-stairs/)

##  题解思路

**动规五步**

- 确定dp[i]的含义：爬到i层楼梯有dp[i]种方法
- 确定递推公式：dp[i] = dp[i - 1] + dp[i - 2]
- 递推数组如何初始化：依据题目可以认为dp[1] = 1 dp[2] = 2
- 确定遍历顺序：可以确定是从底向上去走，从前往后遍历
- 举例推导：

**复杂度分析**

- 时间复杂度：O(n)
- 空间复杂度：O(1)

## 示例代码

```C++
class Solution {
public:
    int climbStairs(int n) {
        vector<int> dp(n + 1);
        if (n <= 2) return n;
        dp[1] = 1;
        dp[2] = 2;
        for (int i = 3; i <= n; i++){
            dp[i] = dp[i - 1] + dp[i - 2];
        }
        return dp[n];
    }
};
//优化
class Solution {
public:
    int climbStairs(int n) {
        if (n <= 1) return n;
        int dp[3];
        dp[1] = 1;
        dp[2] = 2;
        for (int i = 3; i <= n; i++) {
            int sum = dp[1] + dp[2];
            dp[1] = dp[2];
            dp[2] = sum;
        }
        return dp[2];
    }
};
```

# [746. 使用最小花费爬楼梯 ](https://leetcode.cn/problems/min-cost-climbing-stairs/)

## 题解思路

**动规五步**

- 确定动规的dp数组下标含义：到达dp[i]所花费的体力
- 确定递推公式：dp[i] = dp[i - 1] + dp[i  - 2]
- dp数组初始化：只初始化dp[0] 和 dp[1]，后续的通过递推得到
- 确定遍历顺序：从前往后
- 打印dp数组

**复杂度分析**

- 时间复杂度：O(n)
- 空间复杂度：O(n)

## 示例代码

```C++
class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {
        vector<int> dp(cost.size() + 1);
        dp[0] = 0;
        dp[1] = 0;
        for (int i = 2; i <= cost.size(); i++){
            dp[i] = min(dp[i - 1] + cost[i - 1], dp[i - 2] + cost[i - 2]);
        }
        return dp[cost.size()];
    }
};
```

