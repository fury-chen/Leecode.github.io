# [139. 单词拆分 ](https://leetcode.cn/problems/word-break/)

## 题解思路

**背包问题**

- 问题转化思路：将题目视为使用word装满s这个背包的情况，并且以一定的排序来装满

- dp数组含义：字符串长度为i的话dp[i]为true，表示可以拆分成一个或多个在字典中出现的单词
- 确定递推公式：如果确定dp[j] 是true，且[j, i]区间内的子串出现在字典里，那么dp[i]一定时true（j < i)
- dp数组初始化：从递推公式可以看出dp[i]的状态依靠dp[j]是否为true，因此dp[0]=true 
- 确定遍历顺序：题目中强调的是字典的顺序
- 举例推导：

**复杂度分析**

- 时间复杂度：O(n^3)，因为substr中返回的子串副本是O(n)复杂度
- 空间复杂度：O(n)

## 示例代码

```C++
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        unordered_set<string> wordSet(wordDict.begin(), wordDict.end());
        vector<bool> dp(s.size() + 1, false);
        dp[0] = true;
        for (int i = 1; i <= s.size(); i++){
            for (int j = 0; j < i; j++){
                string word = s.substr(j, i - j);
                if (wordSet.find(word) != wordSet.end() && dp[j]){
                    dp[i] = true;
                }
            }
        }
        return dp[s.size()];
    }
};
```

