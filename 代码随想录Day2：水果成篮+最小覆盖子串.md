# [904.水果成篮](https://leetcode.cn/problems/fruit-into-baskets/submissions/)
## 题解思路
**1.滑动窗口**<br>
本题思路是双指针滑动窗口:
- 需要一个Hash关系来维护水果种类;
- 当数组当中的key值超过两个的时候开始更新左指针;
- 每次更新左指针都判断一次当前的窗口是否是最大值，直到整个fruits数组被遍历完。
  
总结使用滑动窗口的情况：需要遍历数组来找到最大或最小子序列长度的问题

**2.复杂度分析**<br>
- 时间复杂度：O(n)
- 空间复杂度：O(1)
  
## 示例代码
```C++
  class Solution {
  public:
      int totalFruit(vector<int>& fruits) {
          unordered_map<int, int> type;
          int left = 0, length = 0;
          for (int right = 0; right < fruits.size(); ++right){
              ++type[fruits[right]];//如果没有这个键就创建一个新的键值对，如果有就将值+1
              while (type.size() > 2){
                  auto deElemnt = type.find(fruits[left]);
                  --deElemnt->second;
                  if (deElemnt->second == 0){
                    type.erase(deElemnt);
                  }   
                  ++left;
              }
              length = max(length, right - left + 1);
          }
          return length;
      }
  };
```

# [76最小覆盖子串](https://leetcode.cn/problems/minimum-window-substring/)
## 题解思路
**1.滑动窗口**

**2.复杂度分析**
## 示例代码
