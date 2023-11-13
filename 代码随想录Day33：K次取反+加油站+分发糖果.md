# [1005. K 次取反后最大化的数组和 ](https://leetcode.cn/problems/maximize-sum-of-array-after-k-negations/)

## 题解思路

**贪心算法**

- 局部最优：k次反转，使绝对值最大的负数变成正数，使绝对值最小的正数变成负数

- 全局最优：达到数组和的最大值

- 注意是按照绝对值来排序，按照大小逐个将负数变成正数

  

**复杂度分析**

- 时间复杂度：O(n）
- 空间复杂度：O(1)

## 示例代码

```C++
class Solution {
static bool cmp(int a, int b){
    return abs(a) > abs(b);
}
public:
    int largestSumAfterKNegations(vector<int>& nums, int k) {
        sort(nums.begin(), nums.end(), cmp);
        for (int i = 0; i < nums.size(); i++){
            if (nums[i] < 0 && k > 0){
                nums[i] *= -1;
                k--;
            }
        }
        if (k % 2 == 1){
            nums[nums.size() - 1] *= -1;
        }
        int result = 0;
        for (int element : nums){
            result += element;
        }
        return result;
    }
};
```

# [134. 加油站 ](https://leetcode.cn/problems/gas-station/)

## 题解思路

**贪心算法**

- 全局进行贪心选择（贪心贪的不是很直观）：

  - case1：如果gas总和小于cost总和，无论从哪里开始都不可能完成一圈
  - case2：res[i] = gas[i] - cost[i]为一次剩下的油量，如果累加没有出现负数则继续行驶
  - case3：如果累加的最小值是负数，汽车就要从非0节点出发，从后往前看，哪个节点能把这个负数填平，就把这个负数填平的节点就是出发节点

- 从每个位置的累加和开始贪：

  - 如果总的油量-消耗>= 0则一定可以跑完一圈

  - 先从下标0开始累加rest，如果curSum < 0则说明车到了这里一定开不过去，前面位置的剩余油量不足以过这个点

  - 直接从curSum < 0的下一位开始，跳过最耗油的点（关于长距离累加的讨论）

    ![img](https://code-thinking-1253855093.file.myqcloud.com/pics/20230117170703.png)

  - 局部最优：一旦当前累加rest[i] < 0，则起始位置至少是 i + 1，因为从i之前开始肯定不行

  - 全局最优：找到能够跑一圈的位置

**复杂度分析**

- 时间复杂度：O(n)
- 空间复杂度：O(1)

## 示例代码

```C++
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int curSum = 0;
        int totalSum = 0;
        int start = 0;
        for (int i = 0; i < gas.size(); i++){
            curSum += gas[i] - cost[i];
            totalSum += curSum;
            if (curSum < 0){
                curSum = 0;
                start = i + 1;
            }
        }
        if (totalSum >= 0) return start;
        else {return -1;}
    }
};
```

# [135. 分发糖果 ](https://leetcode.cn/problems/candy/)

## 题解思路

**贪心算法**

- 采用两次贪心的策略：从两侧进行遍历贪心
  - 从前到后：当右边的大时，右边孩子就多一个
  - 从后往前：当左边的大时，左边孩子就多一个（遍历的顺序要利用到已经有的结果）![img](https://code-thinking-1253855093.file.myqcloud.com/pics/20230202102044.png)
  - 保证每一个人有一个的基础上，按照分数分布从两侧进行贪心
  - 最后要在左右孩子比较的情况下取一个最值

**复杂度分析**

- 时间复杂度：O(n）
- 空间复杂度：O(n)

## 示例代码

```C+
class Solution {
public:
    int candy(vector<int>& ratings) {
        int result = 0;
        vector<int> candyNum(ratings.size(), 1);
        for (int i = 1; i < ratings.size(); i++){
            if (ratings[i] > ratings[i - 1]) candyNum[i] = candyNum[i - 1] + 1;
        }
        for (int i = ratings.size() - 2; i >= 0; i--){
            if (ratings[i] > ratings[i + 1]) candyNum[i] = max(candyNum[i], candyNum[i + 1] + 1);
        }
        for (int i = 0; i < candyNum.size(); i++){
            result += candyNum[i];
        }
        return result;
    }
};
```

