# [455. 分发饼干 ](https://leetcode.cn/problems/assign-cookies/description/)

## 题解思路

- 贪心：将问题转换为优先让胃口最大的小孩子吃饱，逐个直到没有饼干可以吃
  - 局部最优：胃口大的小孩吃饱使得饼干没有浪费
  - 全局最优：尽可能多的孩子吃饱
- 时间复杂度：O(nlogn)
- 空间复杂度：O(1)

## 示例代码

```C++
class Solution {
public:
    int findContentChildren(vector<int>& g, vector<int>& s) {
        sort(g.begin(), g.end());
        sort(s.begin(), s.end());
        int count = 0;
        for (int i = g.size() - 1, j = s.size() - 1; i >= 0; i--){
            if (j >= 0 && s[j] >= g[i]){
                count++;
                j--;
            }
        }
        return count;
    }
};
```

# [376. 摆动序列 ](https://leetcode.cn/problems/wiggle-subsequence/)

## 题解思路

- 贪心：计算出现峰值的情况
  - 局部最优：删除单调坡度上的结点（不包含单调坡度两端结点），这个坡度就可有两个局部峰值
  - 全局最优：整个序列有最多的局部峰值
- 三种情况：
  - 双下坡中有平坡
  - 数组首尾两端
  - 单调中有平坡
- 注意：
  - 默认右侧有一个峰值；
  - 只需要统计数组的局部峰值即可
  - 出现平坡的情况：在局部峰值时才将curDiff赋值preDiff
- 时间复杂度：O(n)
- 空间复杂度：O(1)

## 示例代码

```C++
class Solution {
public:
    int wiggleMaxLength(vector<int>& nums) {
        int preDiff = 0;
        int curDiff = 0;
        int result = 1;
        for (int i = 0; i < nums.size() - 1; i++){
            curDiff = nums[i + 1] - nums[i];
            if ((preDiff >= 0 && curDiff < 0) || (preDiff <= 0 && curDiff > 0)){
                result++;
                preDiff = curDiff;
            }
        }
        return result;
    }
};
```

# [53. 最大子数组和](https://leetcode.cn/problems/maximum-subarray/description/)

## 题解思路

- 贪心：不是遇到负数就停止，而是累加为0停止，从0开始累加，而i不会后退，一直截取的是子窗口
  - 局部最优：当前连续和为负数的时候放弃，从下一个元素开始计算连续和
  - 全局最优：选取到最大的连续子数组和
- 思路上接近不断调整子区间起始位置，终止位置就是count取到最大值记录下来
- 时间复杂度：O(n)
- 空间复杂度：O(1)

## 示例代码

```C++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int result = INT32_MIN;
        int count = 0;
        for (int i = 0 ; i < nums.size(); i++){
            count += nums[i];
            if (count > result){
                result = count;
            }
            if (count <= 0) count = 0;
        }
        return result;
    }
};
```

