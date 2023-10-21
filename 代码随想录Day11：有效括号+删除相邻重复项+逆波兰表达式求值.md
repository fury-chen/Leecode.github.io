# [20.有效的括号](https://leetcode.cn/problems/valid-parentheses/)

## 题解思路

**1.栈的应用**

- 栈的特性非常适合做对称匹配类问题，栈先进后出，使得对称位置能够准确的匹配上
- 三种可能情况：
  - 左边括号多余：遍历完字符串，栈不为空，说明左括号没有右括号来匹配
  - 括号类型没有匹配上：匹配字符串遍历过程中发现栈里没有要匹配的字符，说明左括号没有找到能够匹配的字符
  - 右边括号多余：匹配过程中栈已经空了，说明右括号没有匹配字符
- 技巧：匹配左括号的时候，让右括号入栈，只需要比较当前元素和栈顶相不相等即可

**2.复杂度分析**

- 时间复杂度：O(n)
- 空间复杂度：O(n)

## 示例代码

```C++
class Solution {
public:
    bool isValid(string s) {
        if (s.size() % 2 != 0) return false;
        stack<char> st;
        for (int i = 0; i < s.size(); i++){
            if (s[i] == '('){st.push(s[i]);}
            else if (s[i] == '['){st.push(s[i]);}
            else if (s[i] == '{'){st.push(s[i]);}
            
            else if (s[i] == ')' && !st.empty()){
                if (st.top() != '('){return false;}
                else {st.pop();}
            }
            else if (s[i] == ']' && !st.empty()){
                if (st.top() != '['){return false;}
                else {st.pop();}
            }
            else if (s[i] == '}' && !st.empty()){
                if (st.top() != '{'){return false;}
                else {st.pop();}
            }
            else if (i > 0 && st.empty()){return false;}
        }

        return st.empty();
    }
};
```

```C++
class Solution {
public:
    bool isValid(string s) {
        if (s.size() % 2 != 0) return false; // 如果s的长度为奇数，一定不符合要求
        stack<char> st;
        for (int i = 0; i < s.size(); i++) {
            if (s[i] == '(') st.push(')');
            else if (s[i] == '{') st.push('}');
            else if (s[i] == '[') st.push(']');
            // 第三种情况：遍历字符串匹配的过程中，栈已经为空了，没有匹配的字符了，说明右括号没有找到对应的左括号 return false
            // 第二种情况：遍历字符串匹配的过程中，发现栈里没有我们要匹配的字符。所以return false
            else if (st.empty() || st.top() != s[i]) return false;
            else st.pop(); // st.top() 与 s[i]相等，栈弹出元素
        }
        // 第一种情况：此时我们已经遍历完了字符串，但是栈不为空，说明有相应的左括号没有右括号来匹配，所以return false，否则就return true
        return st.empty();
    }
};
```

# [1047. 删除字符串中的所有相邻重复项](https://leetcode.cn/problems/remove-all-adjacent-duplicates-in-string/)

## 题解思路

**1.栈对称匹配问题**

- bug记录：如果将s[i] != st.top()写在st.empty()之前会出现地址错误，在判断||时应当将优先级高的判断放在前面
- 使用栈来进行过匹配：
  - 遇到相同的字符就pop
  - 遇到不同的字符就push

**2.复杂度分析**

- 时间复杂度：O(n)
- 空间复杂度：O(1)

## 示例代码

```C++
class Solution {
public:
    string removeDuplicates(string s) {
        stack<char> st;
        for (int i = 0; i < s.size(); i++){
            if (st.empty() || s[i] != st.top()){
                st.push(s[i]);
            }else {
                st.pop();
            }
        }
        string result = "";
        while (!st.empty()){
            result += st.top();
            st.pop();
        }
        reverse(result.begin(), result.end());
        return result;
    }
};
```

# [150.逆波兰表达式求值](https://leetcode.cn/problems/evaluate-reverse-polish-notation/)

## 题解思路

**1.使用栈来控制运算顺序**

- 逆波兰表达式：后缀表达式，即运算符写在后面。
- 逆波兰表达式优势
  - 去掉括号后表达无歧义
  - 适合用栈操作运算：遇到数字入栈，遇到运算符出栈两个数字进行运算

**2.复杂度分析**

- 时间复杂度：O(n)
- 空间复杂度：O(n)

## 示例代码

```C++
class Solution {
public:
    int evalRPN(vector<string>& tokens) {
        stack<long long> st;
        for (int i = 0; i < tokens.size(); i++){
            if (tokens[i] == "+" || tokens[i] == "-" || tokens[i] == "*" || tokens[i] == "/"){
                long long num1 = st.top();
                st.pop();
                long long num2 = st.top();
                st.pop();
                if (tokens[i] == "+"){st.push(num2 + num1);}
                if (tokens[i] == "-"){st.push(num2 - num1);}
                if (tokens[i] == "*"){st.push(num2 * num1);}
                if (tokens[i] == "/"){st.push(num2 / num1);}
            }else {
                st.push(stoll(tokens[i]));
            }
        }
        int result = st.top();
        st.pop();
        return result;
    }
};
```

