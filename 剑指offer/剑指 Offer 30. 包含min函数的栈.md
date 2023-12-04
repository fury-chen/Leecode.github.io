# [剑指 Offer 30. 包含min函数的栈](https://leetcode.cn/problems/bao-han-minhan-shu-de-zhan-lcof/submissions/)

```C++
class MinStack {
public:
    /** initialize your data structure here. */
    MinStack() {

    }
    
    stack<int> st;
    stack<int> minSt;

    void push(int x) {
        st.push(x);
        if (minSt.empty()){
            minSt.push(x);
        }else {
            minSt.push(min(x, minSt.top()));
        }
    }
    
    void pop() {
        st.pop();
        minSt.pop();
    }
    
    int top() {
        return st.top();
    }
    
    int getMin() {
        return minSt.top();
    }
};

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack* obj = new MinStack();
 * obj->push(x);
 * obj->pop();
 * int param_3 = obj->top();
 * int param_4 = obj->getMin();
 */
```

- 用一个辅助栈来保存最小值，如果每次都放一个元素进辅助栈就不需要另外的变量来记录最小值
- 注意stack定义的作用域，写在MinStack里不算定义

