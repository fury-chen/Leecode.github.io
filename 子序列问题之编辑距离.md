# [392. 判断子序列 ](https://leetcode.cn/problems/is-subsequence/)

## 题解思路

**动态规划**

- dp数组含义：i - 1 and j - 1的最大相等子序列
- 递推公式：当s[i] == s[j]时 就有dp[i] [j] = dp[i - 1] [j - 1]  + 1； 当s[i] != s[j]时有dp[i] [j] = dp[i] [j - 1]
- dp数组初始化：初始化第一行列
- 遍历顺序：从左到右从上到下
- 举例

**复杂度分析**

- 时间复杂度：O(n * m)
- 空间复杂度：O(n * m)

## 示例代码

```C++
class Solution {
public:
    bool isSubsequence(string s, string t) {
        vector<vector<int>> dp(s.size() + 1, vector<int>(t.size() + 1, 0));
        for (int i = 1; i <= s.size(); i++){
            for (int j = 1; j <= t.size(); j++){
                if (s[i - 1] == t[j - 1]) dp[i][j] = dp[i - 1][j - 1] + 1;
                //注意dp数组的含义是i- 1和 j - 1的关系
                else dp[i][j] = dp[i][j - 1];
            }
        }
        return dp[s.size()][t.size()] == s.size();
    }   
};
```

# [115. 不同的子序列](https://leetcode.cn/problems/distinct-subsequences/)

## 题解思路

**动态规划**

- dp数组含义：以i - 1为结尾的s中有以j-1结尾的t 的个数
- 递推公式：判断i-1和j-1的关系
  - 相同：dp[i] [j] = dp[i - 1] [j - 1] + dp[i - 1] [j]（使用s[i - 1]和不使用s[i - 1]的两种情况在内）
  - 不相同
- 初始化：第一行和第一列的初始化dp[i] [0] = 1  dp[0] [j] = 1
- 遍历顺序：上到下，左到右
- 举例

**复杂度分析**

- 时间复杂度：O(n*m)
- 空间复杂度：O(n*m)

## 示例代码

```C++
class Solution {
public:
    int numDistinct(string s, string t) {
        vector<vector<uint64_t>> dp(s.size() + 1, vector<uint64_t>(t.size() + 1));
        for (int i = 0; i <= s.size(); i++) dp[i][0] = 1;//t空字符串时s至少有一种匹配方法
        for (int j = 1; j <= t.size(); j++) dp[0][j] = 0;//当s为空时没有可以匹配的方法
        for (int i = 1; i <= s.size(); i++){
            for (int j = 1; j <= t.size(); j++){
                if (s[i - 1] == t[j - 1]) dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j];
                else dp[i][j] = dp[i - 1][j];
            }
        }
        return dp[s.size()][t.size()];
    }
};
```

# [583. 两个字符串的删除操作 ](https://leetcode.cn/problems/delete-operation-for-two-strings/)

## 题解思路

**动态规划**

- dp数组含义：dp[i] [j]表示以i-1结尾的字符串word1和以j-1结尾的word2想要达到相等，所需删除的元素最少次数
- 递推公式：当word[i -1]和word[j - 1]相同的时候，dp[i] [j] = dp[i - 1] [j - 1]；当不相同时，有三种情况，对三者取最小值
  - 删除word1[i -1]，最少操作次数为dp[i - 1] [j] + 1
  - 删除word2[j - 1]，最少操作次数同样dp[i] [j] + 1
  - 同时删除word1和word2的，最少操作次数+2
- dp数组初始化：从递推公式看出来，dp[i] [0]和dp[0] [j]是一定要初始化的，dp[i] [0] = i dp[0] [j] = j
- 遍历顺序：从前往后
- 举例：

**动规思路2**

- 本题是求最长公共子序列的一个变体，求出最常公共子序列之后，在最长公共子序列之外的所有字符都删除掉就行，最后用两个字符串的总长度-最长公共子序列即可

**复杂度分析**

- 时间复杂度：O(n*m)
- 空间复杂度：O(n * m)

## 示例代码

```C++
class Solution {
public:
    int minDistance(string word1, string word2) {
        vector<vector<int>> dp(word1.size() + 1, vector<int>(word2.size() + 1));
        for (int i = 0; i <= word1.size(); i++){dp[i][0] = i;}//delete i elements to be empty
        for (int j = 0; j <= word2.size(); j++){dp[0][j] = j;}//and so on

        for (int i = 1; i <= word1.size(); i++){
            for (int j = 1; j <= word2.size(); j++){
                if (word1[i - 1] == word2[j - 1]) dp[i][j] = dp[i - 1][j - 1];
                else{
                    dp[i][j] = min(dp[i - 1][j] + 1, dp[i][j - 1] + 1);
                }
            }
        }
        return dp[word1.size()][word2.size()];
    }
};
//method 2
class Solution {
public:
    int minDistance(string word1, string word2) {
        vector<vector<int>> dp(word1.size() + 1, vector<int> (word2.size() + 1, 0));
        int result = 0;
        for (int i = 1; i <= word1.size(); i++){
            for (int j = 1; j <= word2.size(); j++){
                if (word1[i - 1] == word2[j - 1]) dp[i][j] = dp[i - 1][j - 1] + 1;
                else dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
                if (result < dp[i][j]) result = dp[i][j];
            }
        }
        return word1.size() + word2.size() - result * 2;

    }
};
```

# [72. 编辑距离 ](https://leetcode.cn/problems/edit-distance/)

## 题解思路

**动态规划**

- 感觉本质上应该是求最长重复子串到短串的距离

- dp数组含义：以i-1结尾word1和以j-1结尾的word2最少操作次数
- 递推公式：相同不同讨论，相同延续dp[i - 1] [j - 1]，不同的增删替换操作讨论
  - 删除word1操作（添加word2操作）：dp[i] [j] = dp[i - 1] [j] + 1 or dp[i] [j  - 1] + 1 
  - 替换元素操作：dp[i] [j] = dp[i - 1] [j - 1]  + 1
- 初始化dp数组：初始第一排和第一列dp[0] [j] = j  dp[i] [0] = i
- 遍历顺序：从前往后
- 举例： 

## 示例代码

```C++
class Solution {
public:
    int minDistance(string word1, string word2) {
        vector<vector<int>> dp(word1.size() + 1, vector<int>(word2.size() + 1, 0));
        for (int i = 0; i <= word1.size(); i++){dp[i][0] = i;}
        for (int j = 0; j <= word2.size(); j++){dp[0][j] = j;}
        for (int i = 1; i <= word1.size(); i++){
            for (int j = 1; j <=word2.size(); j++){
                if (word1[i - 1] == word2[j - 1]) dp[i][j] = dp[i - 1][j - 1];
                else dp[i][j] = min({dp[i - 1][j - 1], dp[i - 1][j], dp[i][j - 1]}) + 1;
            }
        }
        return dp[word1.size()][word2.size()];
    }
};
```

