# [122. 买卖股票的最佳时机 II](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/)

## 题解思路

**贪心算法**

- 利润分解：将利润分解为每天为单位的维度，将prices分解为每天的利润序列price = (price[i] - price[i - 1])....+(price[1] - price[0])
- 其实这个分析思路理解起来是这样：price[i] - price[i - 1]是假设交易会产生利润，如果产生利润为负数则不参与交易，手上还是拿着这个股票（限定着第一天必须买入，无论多高），如果为正数则参与交易收获利润

**复杂度分析**

- 时间复杂度：O(n)
- 空间复杂度：O(1)

## 示例代码

```C++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int result = 0;
        for (int i = 1; i <= prices.size() - 1; i++){
            result += max(prices[i] - prices[i - 1], 0);
        }
        return result;
    }
};
```

# [55. 跳跃游戏 ](https://leetcode.cn/problems/jump-game/description/)

## 题解思路

**贪心算法**

- 原始思路是每次都跳到数值最大的区域，但是这样就是两层循环，不够简单（每一次找最大值都意味着一次搜索）
- 本题的跳跃位置选择不重要而在于跳跃的范围能否覆盖到终点
  - 局部最优：每次去最大的跳跃范围(max(cover, nums[i] + i))
  - 整体最优：最后整体最大范围覆盖能否到达终点
- 注意循环的条件要被限定成在cover范围内
- 基于本题的理解：遍历数组来查找范围，其范围能够覆盖整个数组即为True

**复杂度分析**

- 时间复杂度：O(n)
- 空间复杂度：O(1)

## 示例代码

```C++
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int cover = 0;
        if (nums.size() == 1) return true;
        for (int i = 0; i <= cover; i++){
            cover = max((nums[i] + i), cover);
            if (cover >= nums.size() - 1) return true;
        }
        return false;
    }
};
```

# [45. 跳跃游戏 II ](https://leetcode.cn/problems/jump-game-ii/)

## 题解思路

**贪心算法**

- 局部最优 ：当前可移动距离尽可能多走，如果还没有到达终点，步数再+1
- 整体最优：一步尽可能多走而达到最少步数
- 方法1：当移动下标到达当前最远距离下标时，步数就要+1，来增加覆盖距离，最后的步数就是最小步数
- 方法2：针对方法1的特殊情况做统一处理，只需要移动到nums.size() - 2的位置即可
- 基于本题的理解：维护最远范围如果最远范围不足以覆盖整个数组，那么就需要继续维护下一个范围，维护范围个数就是跳跃次数

**复杂度分析**

- 时间复杂度：O(n)
- 空间复杂度：O(1)

## 示例代码

```C++
//method 1
class Solution {
public:
    int jump(vector<int>& nums) {
        if (nums.size() == 1 )return 0;
        int curDistance = 0;
        int result = 0;
        int nextDistance = 0;
        for (int i = 0; i < nums.size(); i++){
            nextDistance = max(nums[i] + i, nextDistance);
            if (i == curDistance){
                result++;
                curDistance = nextDistance;
                if (nextDistance >= nums.size() - 1) break;
            }
        }
        return result;
    }
};
```

```C++
class Solution {
public:
    int jump(vector<int>& nums) {
        int curDistance = 0;
        int result = 0;
        int nextDistance = 0;
        for (int i = 0; i < nums.size() - 1; i++){
            nextDistance = max(nums[i] + i, nextDistance);
            if (i == curDistance){
                curDistance = nextDistance;
                result++;
            }
        }
        return result;
    }
};
```

