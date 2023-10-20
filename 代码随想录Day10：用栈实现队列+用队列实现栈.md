# [232.用栈实现队列](https://leetcode.cn/problems/implement-queue-using-stacks/)

## 题解思路

**1.堆栈操作stack<T>**

- top()：返回一个栈顶元素的引用，类型为T&，如果栈为空，则返回未定义值
- push(const T& obj)：将对象副本压入栈，通过调用底层容器push_back()函数完成的
- push(T&& obj)：以移动对象的方式将对象压入栈，调用底层容器的右值引用参数push_back()完成
- pop()：弹出栈顶元素
- empty()：在栈中没有元素的情况下返回true
- emplace()：用传入的参数调用构造函数，在栈顶生成对象
- swap(stack<T> & other_stack)：将当前栈中的元素和参数中的元素交换，参数所包含元素的类型必须和当前栈的相同

**2.要点提炼**

- 对于pop()：栈是LIFO，队列时FIFO所以需要将栈中所有元素弹出找到最底部的一个，但如果都弹出后续pop就找不到元素，所以需要两个栈来完成
- 关于empty需要同时判断Out和In两个栈，因为pop出来的可能在Out里

**3.复杂度分析**

- 时间复杂度：push和empty为O(1)，pop和peek为O(n)
- 空间复杂度：O(1)

## 示例代码

```C++
class MyQueue {
public:
    stack<int> stIn;
    stack<int> stOut;
    MyQueue() {

    }
    
    void push(int x) {
        stIn.push(x);
    }
    
    int pop() {
        if (stOut.empty()){
            while (!stIn.empty()){
                stOut.push(stIn.top());
                stIn.pop();
            }
        }
        int result = stOut.top();
        stOut.pop();
        return result;
    }
    
    int peek() {
        int res = this->pop();
        stOut.push(res);
        return res;
    }
    
    bool empty() {
        return stIn.empty() && stOut.empty();
    }
};
```

# [225. 用队列实现栈](https://leetcode.cn/problems/implement-stack-using-queues/)

## 题解思路

**1.队列操作 queue<T>**

- front()：返回queue中的第一个元素引用，如果queue是常量，就返回一个常引用，如果为空，返回未定义
- back()：返回queue中最后一个元素的引用，同上
- push(const T& obj)：在queue尾部添加一个元素的副本，调用底层容器的push_back() 完成
- push(T&& obj)：以移动的方式在queue的尾部添加元素，通过调用底层容器具有右值引用参数的成员函数push_back()来完成
- pop()：删除queue中的第一个元素
- size()：返回queue中元素个数
- empty()：如果没有元素返回true
- emplace()：用传给emplace的参数调用T的构造函数，在queue的尾部生成对象
- swap(queue<T> &ither_q)：将当前queue的元素和参数queue中的元素交换，需要包含相同类型的元素，也可以调用全局函数模板swap来完成同样的操作

**2.要点**

- 队列时FIFO规则，将一个队列数据导入另一个队列中，数据顺序并不改变，所以queue2完全就是一个备份的作用
- 实现pop操作的时候，栈要pop最后一个，所以先将que1中的元素复制到que2中只剩最后一个，再将result = que1.front()，然后将que2的值全都还给que1，清空que2

**3.复杂度分析**

- 时间复杂度：pop为O(n)，其他为O(1)
- 空间复杂度：O(n)

## 示例代码

```C++
class MyStack {
public:
    queue<int> que1;
    queue<int> que2;
    MyStack() {

    }
    
    void push(int x) {
        que1.push(x);
    }
    
    int pop() {
        int size = que1.size();
        size--;
        while (size--){
            que2.push(que1.front());
            que1.pop();
        }
        int result = que1.front();
        que1.pop();
        que1 = que2;
        while (!que2.empty()){
            que2.pop();
        }
        return result;
    }
    
    int top() {
        return que1.back();
    }   
    
    bool empty() {
        return que1.empty();
    }
};
```

