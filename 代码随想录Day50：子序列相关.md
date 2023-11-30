# [300. 最长递增子序列 ](https://leetcode.cn/problems/longest-increasing-subsequence/)

## 题解思路

**动态规划**

- dp数组含义：截止i下标最长递增子序列的长度
- 递推公式：dp[i] = max(dp[j] + 1, dp[i])
- 初始化：全都初始化为1，自己至少长度为1
- 遍历顺序：后状态依赖前状态，从前往后
- 举例

**复杂度分析**

- 时间复杂度：O(n^2)
- 空间复杂度：O(n)

## 示例代码

```C++
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int len = nums.size();
        int result = 0;
        vector<int> dp(len, 1);
        if (len <= 1) return len;
        for (int i = 1; i < len; i++){
            for (int j = 0; j < i; j++){
                if (nums[i] > nums[j]){
                    dp[i] = max(dp[j] + 1, dp[i]);
                }
            }
            result = max(result, dp[i]);
        }
        return result;
    }

};
```

# [674. 最长连续递增序列 ](https://leetcode.cn/problems/longest-continuous-increasing-subsequence/description/)

## 题解思路

将上一题的思路j 变成i-1就能AC

## 示例代码

```C++
class Solution {
public:
    int findLengthOfLCIS(vector<int>& nums) {
        int len = nums.size();
        int result = 0;
        if (len <= 1) return len;
        vector<int> dp(len, 1);
        for (int i = 1; i < len; i++){
            if (nums[i - 1] < nums[i]) dp[i] = max(dp[i - 1] + 1, dp[i]);
            result = max(result, dp[i]);
        }
        return result;
    }
};
```

# [718. 最长重复子数组](https://leetcode.cn/problems/maximum-length-of-repeated-subarray/)

## 题解思路

**动态规划**

- dp数组含义：使用一个二维dp数组，以i-1结尾的nums1和以j-1结尾的nums2的最长重复子数组的长度
- 递推公式：if (nums1[i] == nums2[j]) dp[i] [j] = dp[i - 1] [j - 1]
- 初始化：dp[i] [0]和dp[0] [j] = 0 在实际上没有意义
- 遍历顺序：从前往后
- 示例：
- 之所以使dp为i- 1和 j - 1，是为了方便开始进行初始化，不需要对开始的一行一列进行遍历

**复杂度分析**

- 时间复杂度：O(n*m)
- 空间复杂度：O(n*m)

## 示例代码

```C++
class Solution {
public:
    int findLength(vector<int>& nums1, vector<int>& nums2) {
        vector<vector<int>> dp(nums1.size() + 1, vector<int>(nums2.size() + 1, 0));
        int result = 0 ;
        for (int i = 1; i <= nums1.size(); i++){
            for (int j = 1; j <= nums2.size(); j++){
                if (nums1[i - 1] == nums2[j - 1])
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                if (dp[i][j] > result) result = dp[i][j];
            }
            
        }
        return result;
    }
};
//compress version
class Solution {
public:
    int findLength(vector<int>& nums1, vector<int>& nums2) {
        vector<int> dp(nums2.size() + 1, 0);
        int result = 0;
        for (int i = 1; i <= nums1.size(); i++){
            for (int j = nums2.size(); j > 0; j--){
                if (nums1[i - 1] == nums2[j - 1]) dp[j] = dp[j - 1] + 1;
                else dp[j] = 0;
                if (dp[j] > result) result = dp[j];
            }
        }
        return result;
    }
};
```

# [1143. 最长公共子序列 ](https://leetcode.cn/problems/longest-common-subsequence/)

## 题解思路

**动态规划**

- dp数组含义：dp[i] [j]为长度[0, i - 1]的字符串text1与长度为[0, j -1]字符串text2的最常公共子序列为dp[i] [j]
- 递推公式：如果text1[i - 1]和text2[j - 1]相同，那么找到了一个公共元素，则dp[i] [j] = dp[i  - 1] [j - 1] + 1；如果不同，则看text1[0, i -2]与text2[0, j -2]的最长公共子序列，取最大，即dp[i] [j] = max(dp[i - 1] [j], dp[i] [j - 1])
- 数组初始化：根据dp数组含义dp[i] [0] 和dp[0] [j]都应该为0
- 遍历顺序：从前往后

**复杂度分析**

- 时间复杂度：O(n * m)
- 空间复杂度：O(n * m)

## 示例代码

```C++
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        vector<vector<int>> dp(text1.size() + 1, vector<int>(text2.size() + 1, 0));
        int result = 0;
        for (int i = 1; i <= text1.size(); i++){
            for (int j = 1; j <= text2.size(); j++){
                if (text1[i -1] == text2[j - 1]) dp[i][j] = dp[i - 1][j - 1] + 1;
                else dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
                if (dp[i][j] > result) result = dp[i][j];
            }
        }
        return result;
    }
};
```

# [1035. 不相交的线](https://leetcode.cn/problems/uncrossed-lines/description/)

## 题解思路

- 本质上是在求最长公共子序列?——要求相同位置不能乱序
- 题解代码和上述相同

## 示例代码

```C++
class Solution {
public:
    int maxUncrossedLines(vector<int>& nums1, vector<int>& nums2) {
        vector<vector<int>> dp(nums1.size() + 1, vector<int>(nums2.size() + 1, 0));
        int result = 0;
        for (int i = 1; i <= nums1.size(); i++){
            for (int j = 1; j <= nums2.size(); j++){
                if (nums1[i - 1] == nums2[j - 1]) dp[i][j] = dp[i - 1][ j - 1] + 1;
                else dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
                if (dp[i][j] > result) result = dp[i][j]; 
            }
        }
        return result;
    }
};
```



# [53. 最大子数组和 ](https://leetcode.cn/problems/maximum-subarray/)

## 题解思路

**动态规划**

- dp数组含义：包括下标在内的最大连续子序列和为dp[i]
- 递推公式：dp[i] = max(dp[i - 1] + nums[i], nums[i])如果加到了负数就重新计数
- 初始化：dp[0] = nums[0] result = dp[0]
- 遍历顺序：从前往后
- 举例：

**复杂度分析**

- 时间复杂度：O(n)
- 空间复杂度：O(n)

## 示例代码

```C++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        if (nums.size() <= 1) return nums[0];
        vector<int> dp(nums.size() + 1, 0);
        dp[0] = nums[0];
        int result = dp[0];
        for (int i = 1; i < nums.size(); i++){
            dp[i] = max(nums[i], dp[i - 1] + nums[i]);
            if (dp[i] > result) result = dp[i];
        }
        return result;
    }
};
```

