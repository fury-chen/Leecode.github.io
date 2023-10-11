# [704 二分查找](https://leetcode.cn/problems/binary-search/submissions/)
## 思路分析
  **1.区间不变量**<br>
    二分法通过在vector对半取下标来进行target查找，思路简单，但容易出错。
    
    - 写法1：左闭右开<br>
    
      ```
      left = 0
      right = nums.size()
      while(left < right)
      if (nums[mid] < target) right = mid
      if (nums[mid] > target) left = mid - 1```
      ```
    
    - 写法2：左闭右闭<br>
    
     ```C++
      left = 0;
      right = nums.size() - 1;
      while (left <= right);
      if (nums[mid] < target) right = mid - 1;
      if (nums[mid] > target) left = mid + 1;
     ```
  **2.防止溢出**
    当数组比较大的时候int 型可能会溢出，解决方法可以通过缩短下标数字来控制范围，或是使用long long 型来防溢出<br>
    ```
    long long mid = (right - left) / 2;
    int mid = left + (right - left) / 2;
    //or use bit calculate;
    long long mid = (right - left) >> 1;
    int mid = left + ((right - left) >> 1);
    ```
  **3.复杂度分析**
    - 时间复杂度：O(logn) —— 查找次数是指数下降n / (2 ^ k)
    - 空间复杂度： O(1)  —— 没有开辟新变量
    
## 示例代码
```C++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left = 0;
        int right = nums.size();
        //left closed right open
        for (int mid = (right - left) / 2; right > left; ){
            mid = left + ((right - left) / 2);
            cout << mid;
            if (nums[mid] == target) {
                return mid;
            }
            if (nums[mid] > target){
                right = mid;
            }
            if (nums[mid] < target){
                left = mid + 1;
            }
        }
        return -1;
    }
};
```
