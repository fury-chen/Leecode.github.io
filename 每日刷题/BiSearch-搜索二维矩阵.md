# [74. 搜索二维矩阵 ](https://leetcode.cn/problems/search-a-2d-matrix/description/?envType=study-plan-v2&envId=top-100-liked)

## 题解思路

- 二维数组的搜索要注意的就是边界条件，从下往上搜索到小于目标值的数，从左往右搜索等于目标值的数，如果没搜到返回FALSE

## 示例代码

```C++
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
      if (matrix.empty() || matrix[0].empty()) return false;
      int length = matrix.size() - 1;
      int width = matrix[0].size() - 1;
      int i = length; 
      int j = 0;
      while (i >= 0 && j <= width){
        if (matrix[i][j] > target) i--;
        else if (matrix[i][j] < target) j++;
        else {return true;}
      }
      return false;
    }
};
```

