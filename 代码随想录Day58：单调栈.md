# [739. 每日温度 ](https://leetcode.cn/problems/daily-temperatures/)

## 题解思路

**单调栈**

- 单调栈解决问题：一维数组需要寻找任一元素的右边或者左边第一个比自己大或小的元素的位置
- 单调栈的复杂度O(n)怎么得来的？——空间换时间，在遍历过程中用一个栈来记录右边第一个比当前元素高的元素；即用一个栈来记录遍历过的元素
- 单调栈需要明确的关键点：
  - 单调栈李存放的元素是什么？——本题存放的是元素的下标i，如果需要使用时，T[i]就能获取
  - 单调栈的元素是递增还是递减？——一般是从栈顶到栈底的顺序，本题中应当是递增的
  - 单调栈的作用：当前遍历元素和栈顶元素做比较
  - 出栈的顺序决定了单调栈的增减性 ，同时也是栈顶到栈底的单调性
  - 栈操作讨论：
    - 遍历元素大于栈顶元素的情况：将栈顶的弹出，将大的下标加入到栈中，计算二者的距离
    - 小于：
    - 等于：将遍历元素加入栈，不需要计算距离
    - 总结起来就是将增的时候记录，并把栈中递减的部分全都弹出

**复杂度分析**

- 时间复杂度：O(n)
- 空间复杂度：O(n)

## 示例代码

```C++
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        stack<int> st;
        vector<int> result(temperatures.size(), 0);
        st.push(0);
        for (int i = 1; i < temperatures.size(); i++){
            if (temperatures[i] <= temperatures[st.top()]){
                st.push(i);
            }else {
                while (!st.empty() && temperatures[i] > temperatures[st.top()]){
                    result[st.top()] = i - st.top();
                    st.pop();
                }
                st.push(i);
            }
        }
        return result;
    }
};
//compact version
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        stack<int> st;
        vector<int> result(temperatures.size(), 0);
        st.push(0);
        for (int i = 1; i < temperatures.size(); i++){
                while (!st.empty() && temperatures[i] > temperatures[st.top()]){
                    result[st.top()] = i - st.top();
                    st.pop();
                }
                st.push(i);
        }
        return result;
    }
};
```

# [496. 下一个更大元素 I ](https://leetcode.cn/problems/next-greater-element-i/)

## 题解思路

**单调栈**

- 选择递增还是递减：递增单调栈
- 结果result数组的初始化：以nums1大小开一个全为-1的数组
- 没有重复元素——使用Hash关系做映射
- 三种情况的讨论：栈顶下标对应值大于小于等于当前遍历元素值

## 示例代码

```C++
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        stack<int> st;
        vector<int> result(nums1.size(), -1); 
        if (nums1.size() == 0) return result;

        unordered_map<int, int> umap;
        for (int i = 0; i < nums1.size(); i++){
            umap[nums1[i]] = i;
        }
        
        st.push(0);
        for (int i = 1; i < nums2.size(); i++){
            while (!st.empty() && nums2[i] > nums2[st.top()]){
                if (umap.count(nums2[st.top()]) > 0){
                    int index = umap[nums2[st.top()]];
                    result[index] = nums2[i];
                }
                st.pop();
            }
            st.push(i);
        }
        return result;
    }
};
```

