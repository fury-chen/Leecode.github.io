# [704 二分查找](https://leetcode.cn/problems/binary-search/submissions/)
## 思路分析
**1.区间不变量**<br>
二分法通过在vector对半取下标来进行target查找，思路简单，但容易出错。<br>
- 写法1：左闭右开
 ``` C++
  left = 0
  right = nums.size()
  while(left < right)
  if (nums[mid] < target) right = mid
  if (nums[mid] > target) left = mid - 1
 ```
    
- 写法2：左闭右闭
``` C++
 left = 0;
 right = nums.size() - 1;
 while (left <= right);
 if (nums[mid] < target) right = mid - 1;
 if (nums[mid] > target) left = mid + 1;
```
     
**2.防止溢出**<br>
当数组比较大的时候int 型可能会溢出，解决方法可以通过缩短下标数字来控制范围，或是使用long long 型来防溢出<br>
 ```C++
  long long mid = (right - left) / 2;
  int mid = left + (right - left) / 2;
  //or use bit calculate;
  long long mid = (right - left) >> 1;
  int mid = left + ((right - left) >> 1);
 ```
    
**3.复杂度分析**<br>
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
```C++
 class Solution {
 public:
     int search(vector<int>& nums, int target) {
         int left = 0;
         int right = nums.size() - 1;
         while (right >= left){
             int mid = left + ((right - left) / 2);
             if (nums[mid] > target){
                 right = mid - 1;
             }
             else if (nums[mid] < target){
                 left = mid + 1;
             }
             else {
                 return mid;
             }
         }
         return -1;
     }
 };
```

***
# [27.移除元素](https://leetcode.cn/problems/remove-element/)
## 思路分析
**1.双指针法**<br>
通过一个快指针和一个慢指针在一个for循环下完成两个for循环的工作，通过牺牲空间来换取时间
- 快指针：寻找新数组的元素，即不包含目标元素的数组
- 慢指针：指向更新新数组下标的位置
- 思考：是否查找 0 1 状态的都能用双指针法去解决？
- 题目要求：在原数组上进行修改
  
**2.复杂度分析**
- 时间复杂度：O(n^2)
- 空间复杂度：O(1)
  
## 示例代码
```C++
//same direction
 class Solution {
 public:
     int removeElement(vector<int>& nums, int val) {
         int slowIndex = 0;
         for (int fastIndex = 0; fastIndex < nums.size(); fastIndex++){
             if (nums[fastIndex] != val){
                 nums[slowIndex++] = nums[fastIndex]; 
             }
         }
         return slowIndex;
     }
 };
```

```C++
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        //against direction
        int leftIndex = 0;
        int rightIndex = nums.size() -1;
        while (rightIndex >= leftIndex){
            while (rightIndex >= leftIndex && nums[leftIndex] != val){
                leftIndex++;
            }
            while (rightIndex >= leftIndex && nums[rightIndex] == val){
                --rightIndex;
            }
            if (rightIndex > leftIndex){
                nums[leftIndex++] = nums[rightIndex--];
            }
            
        }
        return leftIndex;
    }
};
```
