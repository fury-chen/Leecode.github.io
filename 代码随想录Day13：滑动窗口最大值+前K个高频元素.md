# [347. 前 K 个高频元素 ](https://leetcode.cn/problems/top-k-frequent-elements/)

## 题解思路

**1.优先级队列**

- 本来第一个思路是使用一个map来维护前k个元素，但是map的计数和排序不太好处理
- 首先是统计元素的频率，使用一个map来维护出现的元素次数
- 其次是对频率进行排序，使用优先级队列进行排序（堆）
- 使用小顶堆，每次都把小的pop出去剩下的就是大的了，如果每次都pop大的，还要用vec来装载pop出去的

**2.复杂度分析**

- 时间复杂度：O(nlogk)
- 空间复杂度：O(n)

## 示例代码

```c++
class Solution {
public:
    class myComparison{
        public:
            bool operator()(const pair<int, int>& lhs, const pair<int, int>& rhs){
                return lhs.second > rhs.second;
            }//constum priority
    };
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int, int> map;
        for (int i = 0; i < nums.size(); i++){
            map[nums[i]]++;
        }
        priority_queue<pair<int, int>, vector<pair<int, int>>, myComparison> pri_que; 
        for (unordered_map<int, int>::iterator it = map.begin(); it != map.end(); it++){
            pri_que.push(*it);
            if (pri_que.size() > k){
                pri_que.pop();
            }
        }
        vector<int> result(k);
        for (int i = k - 1; i >= 0; i--){
            result[i] = pri_que.top().first;
            pri_que.pop();
        }
        return result;
    }
};
```

# [239. 滑动窗口最大值 ](https://leetcode.cn/problems/sliding-window-maximum/)



## 题解思路

- 需要一个队列，每次移动窗口，队列一进一出，每次移动后队列报告该窗口的最大值
- 不需要维护窗口当中的所有元素，只需要维护有可能成为窗口当中最大值的元素即可，并保证队列当中的元素值是由小到大的。因此维护这个队列单调递减的队列成为单调队列
- 单调队列：每次维护窗口当中最大值，依次单调递减，即每次进入元素与当前front相比，如果大于就代替，小于就放到后一个位置，直到队列满了，然后将超出的最小值弹出

## 示例代码

```C++
class Solution {
private:
    class MyQueue{
        public:
            deque<int> que;
            void pop(int value){
                if (!que.empty() && value == que.front()){
                    que.pop_front();
                }
            }
            void push(int value){
                while (!que.empty() && value > que.back()){
                    que.pop_back();
                }
                que.push_back(value);
            }
            int front(){
                return que.front();
            }
    };
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        MyQueue que;
        vector<int> result;
        for (int i = 0; i < k; i++){
            que.push(nums[i]);
        }
        result.push_back(que.front());
        for (int i = k; i < nums.size(); i++){
            que.pop(nums[i - k]);
            que.push(nums[i]);
            result.push_back(que.front());
        }
        return result;
    }
};
```

