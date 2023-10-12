# [977.有序数组的平方](https://leetcode.cn/problems/squares-of-a-sorted-array/submissions/)
## 题解思路
**1.暴力解法**<br>
直观想法是将数组中所有元素平方过后进行排序就能得到其升序结果，暴力解法方法如下：
```C++
  class Solution {
  public:
      vector<int> sortedSquares(vector<int>& A) {
          for (int i = 0; i < A.size(); i++) {
              A[i] *= A[i];
          }
          sort(A.begin(), A.end()); // 快速排序
          return A;
      }
  };
```
[sort函数文档](https://www.apiref.com/cpp-zh/cpp/algorithm/sort.html)<br>
[快速排序（Hello 算法）](https://www.hello-algo.com/chapter_sorting/quick_sort/)<br>
**2..双指针法**<br>
示例中给的数组是有序的，只是在平方过后可能会变大，只需要判断当前result位置应当是负数平方还是正数平方即可
- if (A[i] * A[i] > A[j] * A[j] ) result[k--] = A[i] * A[i];
- if (A[i] * A[i] < A[j] * A[j] ) reslut[k--] = A[j] * A[j];
  
**3..时间复杂度**<br>
- 暴力解法时间复杂度O(n + nlogn)
- 双指针法时间复杂度O(n)
## 示例代码
```C++
  class Solution {
  public:
      vector<int> sortedSquares(vector<int>& nums) {
          int k = nums.size() - 1;
          vector<int> result(nums.size(), 0);
          for (int i = 0, j = nums.size() - 1; i <= j ;){
              if (nums[i] * nums[i] > nums[j] * nums[j]){
                  result[k--] = nums[i] * nums[i];
                  i++;
              }else {
                  result[k--] = nums[j] * nums[j];
                  j--;
              }
          }
          return result;
      }
  };
```

# [209.长度最小子数组](https://leetcode.cn/problems/minimum-size-subarray-sum/)
## 题解思路
**1.暴力解法**
暴力解法思路是使用两个for循环找符合条件的子序列，时间复杂度为O(n^2)
```C++
  class Solution {
  public:
      int minSubArrayLen(int s, vector<int>& nums) {
          int result = INT32_MAX; // 最终的结果
          int sum = 0; // 子序列的数值之和
          int subLength = 0; // 子序列的长度
          for (int i = 0; i < nums.size(); i++) { // 设置子序列起点为i
              sum = 0;
              for (int j = i; j < nums.size(); j++) { // 设置子序列终止位置为j
                  sum += nums[j];
                  if (sum >= s) { // 一旦发现子序列和超过了s，更新result
                      subLength = j - i + 1; // 取子序列的长度
                      result = result < subLength ? result : subLength;
                      break; // 因为我们是找符合条件最短的子序列，所以一旦符合条件就break
                  }
              }
          }
          // 如果result没有被赋值的话，就返回0，说明没有符合条件的子序列
          return result == INT32_MAX ? 0 : result;
      }
  };
```
**2.滑动窗口（双指针）** <br>
滑动窗口的思路就是不断调节子序列的起始位置和终止位置，从而选出我们想要的子序列，使用双指针将两层for循环的复杂度降低为一个for循环。<br>
本题中要确定以下三点:
- 窗口内是什么？——满足>= s 的长度最小子序列
- 如何控制移动窗口的起始位置？——窗口内值大于s，向前移动缩小
- 如何控制移动窗口的结束位置？——遍历数组指针，for循环索引移动
  
**3.时间复杂度**<br>
- 暴力解法：O(n^2)
- 滑动窗口：O(n)
 
## 示例代码
```C++
  class Solution {
  public:
      int minSubArrayLen(int target, vector<int>& nums) {
          int result = INT32_MAX;
          int sum = 0;
          int i = 0;
          int subLength = 0;
          for (int j = 0; j < nums.size(); j++){
              sum += nums[j];
              //I write this IF at the first time
              while (sum >= target){
                  subLength = j - i + 1;
                  result = result < subLength ? result : subLength;
                  sum -= nums[i++];
              }
          }
          return result == INT32_MAX ? 0 : result;
      }
  };
```

# [59.螺旋矩阵](https://leetcode.cn/problems/spiral-matrix-ii/)
## 题解思路
**1.模拟螺旋**<br>
第二次看还是没思路，只记得一个大概的轮廓，但是细节的部分基本上都没有过多留意，因此本次要整理处理细节。<br>
如何模拟螺旋的过程？——使用两层循环来顺时针从(0, 0)遍历到res[mid][mid]<br>
顺时针填充矩阵的过程：左闭右开
- 填充上行从左到右：x = startx , y = [starty -> n - offset) 
- 填充右列从上到下：x = [startx -> n - offset) , y = n - offset
- 填充下行从右到左：x = n - offset , y = [n - offset -> starty)
- 填充左列从下到上：x = [n - offset -> startx) , y = starty
  
## 示例代码
```C++
  class Solution {
  public:
      vector<vector<int>> generateMatrix(int n) {
          vector<vector<int>> res(n, vector<int>(n, 0));//define 2 dim vec
          int startx = 0, starty = 0;//every circle start position
          int loop = n / 2;//How many loop of circle
          int mid = n / 2;
          int count = 1;
          int offset = 1;//control every length of edge
          int i, j;
          while (loop--){
              i = startx;
              j = starty;
  
              //four for loop to simulate one circle
              for (j = starty; j < n - offset; j++){
                  res[startx][j] = count++;
              }
              for (i = startx; i < n - offset; i++){
                  res[i][j] = count++;
              }
              for ( ; j > starty; j--){
                  res[i][j] = count++;
              }
              for ( ; i > startx; i--){
                  res[i][j] = count++;
              }
              startx++;
              starty++;
              
              offset += 1;
              //offset = n - loop
          }
          if (n % 2){
              res[mid][mid] = count;
          } 
          return res;
      }
  };
```
