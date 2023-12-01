# [647. 回文子串 ](https://leetcode.cn/problems/palindromic-substrings/)

## 题解思路

**动态规划**

- 确定dp数组含义：[i, j ]是否是回文子串
- 递推公式：相同时有三种情况
  - i=j
  - j - 1 < 1
  - j - i > 1
- dp数组初始化：dp全都初始化为false
- 遍历顺序：从左往右，从下往上
- 举例推导：

**复杂度分析**

- 时间复杂度：O(n^2)
- 空间复杂度：O(n^2)

## 示例代码

```C++
class Solution {
public:
    int countSubstrings(string s) {
        int result = 0;
        vector<vector<bool>> dp(s.size(), vector<bool>(s.size(), false));
        for (int i = s.size() - 1; i >= 0; i--){
            for (int j = i; j < s.size(); j++){
                if (s[i] == s[j]){
                    if (j - i <= 1){
                        result++;
                        dp[i][j] = true;
                    }else if (dp[i + 1][j - 1]){
                        result++;
                        dp[i][j] = true;
                    }
                }
            }
        }
        return result;
    }
};
//compact version
class Solution {
public:
    int countSubstrings(string s) {
        vector<vector<bool>> dp(s.size(), vector<bool>(s.size(), false));
        int result = 0;
        for (int i = s.size() - 1; i >= 0; i--){
            for (int j = i; j < s.size(); j++){
                if (s[i] == s[j] && (j - i <= 1 || dp[i + 1][j - 1])){
                    result++;
                    dp[i][j] = true;
                }
            }
        }
        return result;
    }
};
//two pointer merhod
class Solution {
public:
    int extend(const string& s, int i, int j, int n ){
        int res = 0;
        while (i >= 0 && j < n && s[i] == s[j]){
            i--;
            j++;
            res++;
        }
        return res;
    }
    int countSubstrings(string s) {
        int result = 0;
        for (int i = 0; i < s.size(); i++){
            result += extend(s, i, i, s.size());
            result += extend(s, i, i + 1, s.size());
        }
        return result;
    }
};
```



# [516. 最长回文子序列 ](https://leetcode.cn/problems/longest-palindromic-subsequence/)

## 题解思路

**动态规划**

- dp数组含义：dp[i] [j]表示字符串s在[i, j]范围内的最常回文子序列长度为dp[i] [j]
- 确定递推公式：如果s[i] == s[j] 则有dp[i] [j] = dp[i + 1] [j - 1] + 2；如果不相同则取前一个的最大值
- 初始化：dp[i] [i] = 1因为中间单个数值都是自己的回文子串
- 遍历顺序：从下往上，从左往右
- 举例说明：

**复杂度分析**

- 时间复杂度：O(n^2)
- 空间复杂度：O(n^2)

## 示例代码

```C++
class Solution {
public:
    int longestPalindromeSubseq(string s) {
        vector<vector<int>> dp(s.size(), vector<int>(s.size(), 0));
        for (int i = 0; i < s.size(); i++) dp[i][i] = 1;

        for (int i = s.size() - 1; i >= 0; i--){
            for (int j = i + 1; j < s.size(); j++){
                if (s[i] == s[j]){
                    dp[i][j] = dp[i + 1][j - 1] + 2;
                }else {
                    dp[i][j] = max(dp[i + 1][j], dp[i][j - 1]);
                }
            }
        }
        return dp[0][s.size() - 1];
    }
};
```

