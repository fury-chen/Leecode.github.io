# [344. 反转字符串 ](https://leetcode.cn/problems/reverse-string/)

## 题解思路

**1.双指针法**：

- 两个指针相向而行，使用一个临时变量完成交换或是直接使用swap函数
- 使用reserve函数要知道其底层原理

**2.复杂度分析**

- 时间复杂度：O(n）
- 空间复杂度：O(1)



## 示例代码

```C++
class Solution {
public:
    void reverseString(vector<char>& s) {
        int left = 0;
        int right = s.size() - 1;
        while (right > left)
        {
            char tmp;
            tmp = s[left];
            s[left] = s[right];
            s[right] = tmp;
            left++;
            right--;
        }
        
    }
};
//Carl's compact version
class Solution {
public:
    void reverseString(vector<char>& s) {
        for (int i = 0, j = s.size() - 1; i < s.size()/2; i++, j--) {
            swap(s[i],s[j]);
        }
    }
};

```

# [541. 反转字符串 II ](https://leetcode.cn/problems/reverse-string-ii/)

## 题解思路



## 示例代码

```C++
//constum reverse function
class Solution{
    void reverse(string s, int start, int end){
        for (int i = start, j = end; i < j; i++, j--){
            swap(s[i], s[j]);
        }
    }
    string reverseStr(string s, int k){
        for (int i = 0; i < s.size(); i += (2 * k)){
            if (i + k <= s.size()){
                reverse(s, i, i + k);
                continue;
            }else {
                reverse(s, i, s.size() - 1);
            }
        }
        return s;
    }
}
```



```C++
class Solution {
public:
    string reverseStr(string s, int k) {
        if (k == 1) return s;
        for (int i = 0; i < s.size(); i += (2 * k)){
            if (i + k <= s.size()){
                reverse(s.begin() + i, s.begin() + i + k);
            }else {
                reverse(s.begin() + i, s.end());
            }
        }
        return s;
    }
};


```

# 剑指offer 05.替换空格

## 题解思路

**1.快慢指针**

- 先检测字符串有多少个空格，将字符串进行扩容到空格替换后的大小

- 使用快慢指针，如果遇到空格位置就填入替换字符串，反之则原地移动

- 快慢指针初始化值为字符串改变前后大小

  

**2.复杂度分析**

- 时间复杂度：O(n)

- 空间复杂度：O(1)

  

**3.数组填充类问题**

- 操作方法是预先给数组扩容带填充后的大小，然后从后开始向前移动

- 这样有两个好处：不用申请新的数组；从后向前避免了数组后移的操作

- 字符串与数组的区别：

  - 字符串是若干字符组成的优先序列，也可以理解为一个字符数组，但对其有特殊规定

  - C：把一个字符串存入一个数组的时候，也把结束符'\0'存入数组，并以此判断字符串是否结束的标志

  - C++：提供一个string类时会有size接口，用于判断string是否结束，不需要'\0'来判断

    

## 示例代码

```C++
class Solution{
public:
    void replaceSpace(string s){
        int count = 0;
        int sOldSize = s.size();
        for (int i = 0; i < sOldSize; i++){
            if (s[i] == ' '){
                count++;
            }
        }
        s.resize(sOldSize + 2 * count);
        int newSize = s.size();
        for (int i = newSize - 1, j = sOldSize - 1; j < i; i--, j++){
            if (s[j] != ' '){s[i] = s[j];}
            if (s[j] == ' '){
                s[i] = '0';
                s[i - 1] = '2';
                s[i - 2] = '%';
                i -= 2;
            }
            
        }
        return s;
    }
};

```



# [151. 反转字符串中的单词](https://leetcode.cn/problems/reverse-words-in-a-string/)

## 题解思路

**1.反转再反转**

- 要求：不使用辅助额外空间
- 移除多余空格：快慢指针法，遇到多余空格字符就补位

```C++
void removeExtraSpace(string& s){
    int slowIndex = 0, fastIndex = 0;
    while (s.size() > 0 && fastIndex < s.size() && s[fastIndex] == ' '){
        fastIndex++;// find first word
    }
    for (; fastIndex < s.size(); fastIndex++){
        if (fastIndex - 1 > 0 && s[fastIndex - 1] == s[fastIndex] && s[fastIndex] == ' '){
            continue;
        }else {
            s[slowIndex++] = s[fastIndex];
        }
    }
    if (slowIndex - 1 > 0 && s[slowIndex - 1] == ' '){
        s.resize(slowIndex - 1);
    }else{
        s.resize(slowIndex);
    }
}
//compact version
void removeExtraSpace(string& s){
    int slow = 0;
    for (int i = 0; i < s.size(); ++i){
        if (s[i] != ' '){
            if (slow != 0) s[slow++] = ' ';
            while (i < s.size()){
                s[slow++] = s[i++];
            }
        }
    }
    s.resize(slow);
}
```



- 反转整个字符串
- 反转每个单词：单词的反转依据是根据单词末尾的空格

**2.复杂度分析**

- 时间复杂度：O(n)
- 空间复杂度：O(1)

## 示例代码

```C++
class Solution {
public:
    void reverse(string& s, int start, int end){
        for (int i = start, j = end; i < j; i++, j--){
            swap(s[i], s[j]);
        }
    }

    void removeExtraSpace(string& s){
        int slow = 0;
        for (int i = 0; i < s.size(); ++i){
            if (s[i] != ' '){
                if (slow != 0) s[slow++] = ' ';
                while (i < s.size() && s[i] != ' '){
                    s[slow++] = s[i++];
                }
            }
        }
        s.resize(slow);
    }

    string reverseWords(string s) {
        removeExtraSpace(s);
        reverse(s, 0, s.size() - 1);
        int start = 0;
        for (int i = 0; i <= s.size(); ++i){
            if (i == s.size() || s[i] == ' '){
                reverse(s, start, i - 1);
                start = i + 1;
            } 
        }
        return s;
    }
};
```

# 左旋字符串

## 题解思路

**1.反转再反转**

- 反转区间前n的子串
- 反转区间n到末尾的子串
- 反转整个字符串

**2.复杂度分析**

- 时间复杂度：O(n)
- 空间复杂度：O(1)

## 示例代码

```C++
class Solution{
    string reverseLeftWords(string s, int n){
        reverse(s, s.begin(), s.begin() + n);
        reverse(s, s.begin() + n, s.end());
        reverse(s, s.begin(), s.end());
        return s;
    }
};
```



# 今日总结：

**1.如何能够在字符串原本空间修改字符串（不申请新的空间）？**

- 使用resize来控制字符串的长度
- 通过局部旋转和整体旋转来达到一定的翻转要求
- 使用双指针法来将字符串覆盖

**2.字符串反转代码要能自己写，不要过度依赖库函数**

```C++
void reverse(string& s, int start, int end){
    for (int i = start, j = end; i < j; i++, j--){
        swap(s[i], s[j]);
    }
}
```

**3.双指针去除多余空格**

- 前面部分：通过fastIndex来找第一个单词，只要不是空格就开始进入句子
- 中间部分可以这么记：fastIndex负责找不是空格的字符，slowIndex负责停留在空格字符来等着被覆盖
- 末尾部分：如果slowIndex的前一个字符是空格就size- 1，反之就是size

```C++
void removeExtraSpace(string& s){
    int fastIndex = 0, slowIndex = 0;
    while (s.size() > 0 && fastIndex < s.size() && s[fastIndx] = ' '){
        fastIndex++;//find first not space char
    }
    //fronthead space remove
    for (; fastIndex < s.size(); fastIndex++){
        if (fastIndex - 1 > 0 && s[fastIndex - 1] == s[fastIndex] && s[fastIndex] == ' '){
            continue;// if fastIndex meet extra spaces we continue
        }else {
            s[slowIndex++] = s[fastIndex]
        }
    }
    //middle words space remove
    if (slowIndex - 1 > 0 && s[slowIndex - 1] == ' '){
        s.resize(slowIndex - 1);
    }else {
        s.resize(slowIndex);
    }
    //tail spaces remove
}
//compact version
void removeExtraSpace(string& s){
    int slow = 0;
    for (int i = 0; i < s.size(); ++i){
        if (s[i] != ' '){//fronthead
            if (slow != 0) s[slow++] = ' ';//word end
            while (i < s.size() && s[i] != ' '){s[slow++] = s[i++];}//middle
        }
    }
}
```

