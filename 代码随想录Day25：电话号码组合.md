# [17. 电话号码的字母组合 ](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/description/)

## 题解思路

**1.数字与字母的映射**

- 可以使用Map或者一个二维数组做一个映射（二维数组其实也是个表格）
- 将静态映射的关系使用一个表格来存放是常见的思路

**2.回溯法**

- 确定回溯函数参数：使用一个result来收集符合要求的结果，使用一个index来记录遍历到第几个数字，同时也表示树的深度
- 确定终止条件：到达了digits的深度，就收集结果
- 确定单层递归逻辑：首先取index指向的数字（类似勒索信的字符串匹配）
- 注意点：
  - 本题每个数字代表的是不同的集合，也即求不同集合之间的组合；
  - 注意某些特殊字符的输入，如1 # 等；

**3.复杂度分析**

- 时间复杂度：O(3^m*4^n)
- 空间复杂度：O(3^m*4^n)

## 示例代码

```C++
class Solution {
public:
    const string letterMap[10] = {
        "",//0
        "",//1
        "abc",//2
        "def",//3
        "ghi",//4
        "jkl",//5
        "mno",//6
        "pqrs",//7
        "tuv",//8
        "wxyz"//9
    };
    vector<string> result;
    string s;
    void backtracking(const string& digits, int startIndex){
        if (startIndex == digits.size()){
            result.push_back(s);
            return;
        }
        int digit = digits[startIndex] - '0';
        string letters = letterMap[digit];
        for (int i = 0; i < letters.size(); i++){
            s.push_back(letters[i]);
            backtracking(digits, startIndex + 1);
            s.pop_back();
        }
    }
    vector<string> letterCombinations(string digits) {
        s.clear();
        result.clear();
        if (digits.size() == 0){return result;}
        backtracking(digits, 0);
        return result;
    }
};
```

