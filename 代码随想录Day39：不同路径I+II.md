# [62. 不同路径 ](https://leetcode.cn/problems/unique-paths/)

## 题解思路

**动态规划**

- 确定dp下标的含义：从(0, 0)位置到(i, j)位置有多少种路径
- 确定递推公式：求解dp[i] [j] = dp[i - 1] [j]  + dp[i] [j - 1]
- dp数组的初始化：从(0, 0)位置到(i, 0)的路径只有一条，则(0, j)也同理，即将行列第一都初始化为1
- 确定遍历顺序：从(0, 0 ) 到（m, n)
- 举例推导：
- 理解：抽象成爬楼梯问题，就相当于每次只能走一步，但是是二维方向，由于只能走一步，所以是两个维度的前置位置之和
- 优化思路：将二维数组按照行来更新dp[j] = dp[j] + dp[j - 1]

**复杂度分析**

- 时间复杂度：O(m*n)
- 空间复杂度：O(m*n) 优化为O(n)

## 示例代码

```C++
class Solution {
public:
    int uniquePaths(int m, int n) {
        vector<vector<int>> dp(m, vector<int>(n, 0));
        for (int i = 0; i < m; i++){
            dp[i][0] = 1;
        }
        for (int j = 0; j < n; j++){
            dp[0][j] = 1;
        }
        for (int i = 1; i < m; i++){
            for (int j = 1; j < n; j++){
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
            }
        }
        return dp[m - 1][n - 1];
    }
};

//一维数组优化
class Solution {
public:
    int uniquePaths(int m, int n) {
        vector<int> dp(n, 1);
        for (int j = 1; j < m; j++){
            for (int i = 1; i < n; i++){
                dp[i] += dp[i - 1];
            }
        }
        return dp[n - 1];
    }
};
```

# [63. 不同路径 II ](https://leetcode.cn/problems/unique-paths-ii/)

## 题解思路

**动态规划**

- 明确dp的含义：同上文，是到达(i, j)的路径数量
- 确定递推公式：同上文
- dp初始化：判断是否有障碍物，有障碍物后续都为0，没有障碍物则初始化为 1
- 遍历顺序：从1 - m/n
- 举例网格

**复杂度分析**

- 时间复杂度：O(m*n)
- 空间复杂度：O(m*n)

## 示例代码

```C++
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        int m = obstacleGrid.size();
        int n = obstacleGrid[0].size();
        if (obstacleGrid[m - 1][n - 1] == 1 || obstacleGrid[0][0] == 1)return 0;
        vector<vector<int>> dp(m, vector<int>(n, 0));
        for (int i = 0; i < m && obstacleGrid[i][0] == 0; i++) dp[i][0] = 1;
        for (int j = 0; j < n && obstacleGrid[0][j] == 0; j++) dp[0][j] = 1;
        for (int i = 1; i < m; i++){
            for (int j = 1; j < n; j++){
                if (obstacleGrid[i][j] == 1) continue;
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
            }
        }
        return dp[m -1][n - 1];
    }
};
//优化版本
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        if (obstacleGrid[0][0] == 1) return 0;
        vector<int> dp(obstacleGrid[0].size());
        for (int j = 0; j < dp.size(); j++){
            if (obstacleGrid[0][j] == 1) dp[j] = 0;
            else if (j == 0) dp[j] = 1;
            else dp[j] = dp[j - 1];
        }
        for (int i = 1; i < obstacleGrid.size(); ++i){
            for (int j = 0; j < dp.size(); ++j){
                if (obstacleGrid[i][j] == 1)dp[j] = 0;
                else if (j != 0) dp[j] += dp[j - 1]; 
            }
        }
        return dp.back();
    }
};
```

