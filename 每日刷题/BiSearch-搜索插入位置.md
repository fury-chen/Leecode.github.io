# [35. 搜索插入位置 ](https://leetcode.cn/problems/search-insert-position/description/?envType=study-plan-v2&envId=top-100-liked)

## 题解思路

- 标准二分查找方法，时间复杂度为O(logn)，注意本题的左右区间开闭关系
- 本解法采用的是左闭右闭，如果没有查找到则返回左区间

## 示例代码

```C++
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
      int n = nums.size();
      int left = 0, right = n - 1;
      while (right >= left){
        int mid = left + (right - left) / 2;
        if (nums[mid] < target) left = mid + 1;
        else right = mid - 1;
      }
      return left;
    }
};
```

