# [860. 柠檬水找零](https://leetcode.cn/problems/lemonade-change/description/)

## 题解思路

**贪心算法**

- 情况一：账单是5，直接收下。
- 情况二：账单是10，消耗一个5，增加一个10
- 情况三：账单是20，优先消耗一个10和一个5，如果不够，再消耗三个5
- 所以局部最优：遇到账单20，优先消耗美元10，完成本次找零。
- 全局最优：完成全部账单的找零。

## 示例代码

```C++
//My AC code
class Solution {
public:
    bool lemonadeChange(vector<int>& bills) {
        int fiveCount = 0;
        int tenCount = 0;
        for (int i = 0; i < bills.size(); i++){
            if (bills[i] == 5){
                fiveCount++;
            }
            if (bills[i] == 10){
                tenCount++;
                fiveCount--;
            }
            if (bills[i] == 20){
                if (tenCount > 0){
                    tenCount--;
                    fiveCount--;
                }
                else {
                    fiveCount -= 3;
                }
            }        
            if (fiveCount < 0) return false;
            if (tenCount < 0) return false;
        }

        return true;
    }
};
```

```C++
//Carl's AC code
class Solution {
public:
    bool lemonadeChange(vector<int>& bills) {
        int five = 0, ten = 0, twenty = 0;
        for (int bill : bills) {
            // 情况一
            if (bill == 5) five++;
            // 情况二
            if (bill == 10) {
                if (five <= 0) return false;
                ten++;
                five--;
            }
            // 情况三
            if (bill == 20) {
                // 优先消耗10美元，因为5美元的找零用处更大，能多留着就多留着
                if (five > 0 && ten > 0) {
                    five--;
                    ten--;
                    twenty++; // 其实这行代码可以删了，因为记录20已经没有意义了，不会用20来找零
                } else if (five >= 3) {
                    five -= 3;
                    twenty++; // 同理，这行代码也可以删了
                } else return false;
            }
        }
        return true;
    }
};
```

# [406. 根据身高重建队列 ](https://leetcode.cn/problems/queue-reconstruction-by-height/)

## 题解思路

**贪心算法**

- 和分发糖果的思路类似，如果从两个维度去思考则必然会顾此失彼，优先考虑一个维度，再根据另一个维度进行排列
- 如果按照k去排列，发现身高不符合条件并且人数也不符合条件，其根本原因是k本身是基于h来排列的，h才是根本属性
- 则转而先对h进行排序，排序之后，如果k不符合，则按照k为下标重新插入队列就行了
- 局部最优：优先按照身高来插入，插入过后people满足队列属性
- 全局最优：最后都做完插入，满足属性

**复杂度分析**

- 时间复杂度：O(nlogn+n^2)
- 空间复杂度：O(n)

## 示例代码

```C++
class Solution {
public:
    static bool cmp(const vector<int>& a, const vector<int> b){
        if (a[0] == b[0]) return a[1] < b[1];
        return a[0] > b[0];
    }
    vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
        sort(people.begin(), people.end(), cmp);
        vector<vector<int>> que;
        for (int i = 0; i < people.size(); i++){
            int position = people[i][1];
            que.insert(que.begin() + position, people[i]);
        }
        return que;
    }
};
```

# [452. 用最少数量的箭引爆气球](https://leetcode.cn/problems/minimum-number-of-arrows-to-burst-balloons/)

## 题解思路

**贪心算法**

- 每次都射的覆盖最多的位置，如果没有重叠的位置就+1，如果有重叠就到下一个
- 排序过后该重叠重叠

**复杂度分析**

- 时间复杂度：O(nlogn)
- 空间复杂度：O(1)

## 示例代码

```C++
class Solution {
private:
    static bool cmp(const vector<int>& a, const vector<int>& b){
        return a[0] < b[0];
    }
public:
    int findMinArrowShots(vector<vector<int>>& points) {
        if (points.size() == 0) return 0;
        sort(points.begin(), points.end(), cmp);
        int result = 1;
        for (int i = 1; i < points.size(); i++){
            if (points[i][0] > points[i - 1][1]){
                result++;
            }else {
                points[i][1] = min(points[i - 1][1], points[i][1]);
            }
        }
        return result;
    }
};
```

