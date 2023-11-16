# [452. 用最少数量的箭引爆气球 ](https://leetcode.cn/problems/minimum-number-of-arrows-to-burst-balloons/)

## 题解思路

- 排序：按照左右边界来排序都是可取的，写一个排序的cmp函数
- 重合：下一个气球的左边界< 上一个气球的右边界，多用一支箭
- 多个重合：需要一个右边界的最小值来更新重叠区域

**贪心思路**

- 局部最优：重叠就射爆，不增加箭数
- 全局最优：使用最少的箭，统计出了重叠的数目

## 示例代码

```C++
class Solution {
public:
    static bool comp(const vector<int>& a, const vector<int>& b){
        return a[0] < b[0];//right side boundary weak order
    }
    int findMinArrowShots(vector<vector<int>>& points) {
        if (points.size() == 0) return 0;
    int count = 1;
        sort(points.begin(), points.end(), comp);
        for (int i = 1; i < points.size(); i++){
            if (points[i][0] > points[i - 1][1]){
                count++;
            }else {
                points[i][1] = min(points[i - 1][1], points[i][1]);
            }
        }
        return count;
    }
};
```



# [435. 无重叠区间 ](https://leetcode.cn/problems/non-overlapping-intervals/)

## 题解思路

**贪心算法**

- 根据上一道打爆气球的做法，将重叠部分进行统计
- 处理上还是要记最小的重叠右区间

## 示例代码

```C++
class Solution {
public:
    static bool cmp(const vector<int>& a, const vector<int>& b){
        return a[1] < b[1];
    }
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {
        if (intervals.size() == 0) return 0; 
        sort(intervals.begin(), intervals.end(), cmp);
    int count = 1;
    int end = intervals[0][1];
        for (int i = 1; i < intervals.size(); i++){
            if (end <= intervals[i][0]){
                end = intervals[i][1];
                count++;
            }
        }
        return intervals.size() - count;
    }
};
```

# [763. 划分字母区间 ](https://leetcode.cn/problems/partition-labels/)

## 题解思路

- 记录字符串的每个字符的最远位置，即通过遍历字符串，将所有字符的所要包含的范围都做了一个统计
- 再遍历一次字符串，根据最远位置，将分界线放到结果集中

**复杂度分析“”

- 时间复杂度：O(n)
- 空间复杂度：O(1)

## 示例代码

```C++
class Solution {
public:
    vector<int> partitionLabels(string s) {
        int hash[27] = {0};
        for (int i = 0; i < s.size(); i++){
            hash[s[i] - 'a'] = i;
        }
    vector<int> result;
    int left = 0;
    int right = 0;
        for (int i = 0; i < s.size(); i++){
            right = max(right, hash[s[i] - 'a']);
            if (i == right){
                result.push_back(right - left + 1);
                left = i + 1;
            }
        }
        return result;
    }

};
```

# [56. 合并区间 ](https://leetcode.cn/problems/merge-intervals/description/)



## 题解思路

- 重叠区间问题：先排序，检查左右边界的关系（重叠条件都一样），只和前一个区间的右边界去比较（按照左边界排序后）；注意本题的左右区间重合条件带=
- 不要在原数组上进行删改操作

**复杂度分析**

- 时间复杂度：O(nlogn)
- 空间复杂度：O(logn)）（排序开销）

## 示例代码

```C++
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        vector<vector<int>> result;
        if (intervals.size() == 0) return result;
        sort(intervals.begin(), intervals.end(), [] (const vector<int>& a, const vector<int>& b){ return a[0] < b[0];});
        result.push_back(intervals[0]);
        for (int i = 1; i < intervals.size(); i++){
            if (result.back()[1] >= intervals[i][0]){
                result.back()[1] = max(result.back()[1], intervals[i][1]);
            }else {
                result.push_back(intervals[i]);
            }
        }
        return result;
    }
};
```

