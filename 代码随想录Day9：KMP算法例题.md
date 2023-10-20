# KMP算法介绍

**1.KMP算法有什么用**

- KMP算法主要思想是出现字符串不匹配时，可以知道已经匹配的文本内容，并用这些信息避免从头再去做匹配（思想有点像动态规划？）
- 如何记录已经匹配的文本内容？（Hash为什么不是优选？因为Hash不具备连续信息，只能理解字母不能理解单词）KMP算法使用的是next数组来记录文本内容

**2.KMP算法怎么用**

- 前缀表(Prefix Table)是什么？——用来回退，记录了模式串与主串不匹配的时候，模式串应当从哪里开始重新匹配。前缀表当中记录的是下标i之前（包括i）的字符串中，有多大长度的相同前缀后缀
- 前后缀是什么？——前缀指的是不包含最后一个字符的所有以第一个字符开头的连续子串；后缀指的是不包含第一个字符的所有以最后一个字符结尾的连续子串
- 为什么要是用前缀表？前缀表记录着前后相同，后续相同的位置可以直接作为开始
- 计算前缀表的方式？
  - [aabaaf]
  - [010120]
- 前缀表与next数组：next数组既可以是前缀表也可是前缀表整体-1
- 时间复杂度分析：
  - 其中n为文本串长度，m为模式串长度，整个KMP算法的时间复杂度为O(n+m)
  - 暴力解法为O(n*m)

```
总结起来就是，KMP算法是使用前缀表来降低文本匹配的时间复杂度的算法，前缀表记录了前后相同子串的长度，避免了从头回退的浪费。
将KMP算法拆解成两个部分来记，第一部分是怎么得到next数组，第二部分是next数组怎么使用
```



# [28. str()Str()实现](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/)

## 题解思路

**1.next数组**

- 初始化：为了确定是从-1开始还是0开始
- 处理前后缀不相同的情况：前后缀不同的时候就回退到上一个，因为next找的是子串的匹配，每一次后移都是子串的更新，又要将子串的所有字符进行匹配，所以要用while不断更新
- 处理前后缀相同的情况：前后缀相同的时候就将相同前后缀的索引+1

**2.使用next数组**

- 当遇见不匹配的时候，就寻找之前匹配的位置
- 遇见匹配的时候，就将匹配串数量++
- 直到模式串末尾，返回下标

**3.复杂度分析**

- 时间复杂度：O(n+m)
- 空间复杂度：O(1)

## 示例代码

```C++
class Solution {
public:
    void getNext(int* next, string& s){
        int j = -1;                         //回退一个的处理方式
        next[0] = j;                        //从-1开始
        for (int i = 1; i < s.size(); i++){ //i从1开始进行前缀匹配
            while(j >= 0 && s[i] != s[j + 1]){
                j = next[j];                //向前回退，至少回退一个单位
            }
            if (s[i] == s[j + 1]){
                j++;
            }
            next[i] = j;//将j（前缀长度）赋给nx[i]
        }
    }
    int strStr(string haystack, string needle) {
        if (needle.size() == 0){
            return 0;
        }
        int next[needle.size()];
        getNext(next, needle);
        int j = -1;     //next数组中的起始位置为-1
        for (int i = 0; i < haystack.size(); i++){
            while (j >= 0 && haystack[i] != needle[j + 1]){
                j = next[j];//j寻找之前匹配的位置   
            }
            if (haystack[i] == needle[j + 1]){
                j++;
            }
            if (j == (needle.size() - 1)){
                return (i - needle.size() + 1);
            }
        }
        return  -1;
    }
};
```

# [459.重复的子字符串](https://leetcode.cn/problems/repeated-substring-pattern/)

## 题解思路

**1.暴力解法**

- for循环获取子串的终止位置，判断子串能否重复构成子字符串
- 嵌套一个for循环，来判断是否整个由重复组成

```C++
for (int i in s){
    needle = splite(0, i)
    for j = 0; j < s.size(); j += needle.size()
        if (needle[j] != s[j])  return false;
}
```

**2.移动匹配**

- 前后子串如果是一致的，那么必有前后相+，还能够组成原本的字符串，则说明是由重复子串组成的
- 在判断s+s拼接的字符串是否出现一个s的时候，首先要排除s+s的首字符和尾字符，防止搜索出原本的字符串

**3.KMP算法**

- 最长相等前后缀与重复子串的关系？——在由重复字串组成的字符串中，最长相等前后缀不包含的子串就是最小重复子串（由前缀后缀的定义产生的错位导致的性质，前缀包含最小重复子串的一半，后缀包含另一半，如果二者相等，则说明整个是由重复字串构成的）；Carl使用的是递推关系
- 如果最长相等前后缀的长度错位长度能够被整除，则说明有重复字串

- 错误：老是将getNext中的while写成if，忘了这个子串是新的要不停倒退

## 示例代码

```C++
//KMP solution
class Solution {
public:
    void getNext(int* next, const string& s){
        next[0] = -1;
        int j = -1;
        for (int i = 1; i < s.size(); i++){
            while (j >= 0 && s[i] != s[j + 1]){
                j = next[j];
            }
            if (s[i] == s[j + 1]){
                j++;
            }
            next[i] = j;
        }
    }
    bool repeatedSubstringPattern(string s) {
        if (s.size() == 0){return false;}
        int next[s.size()];
        getNext(next, s);
        int len = s.size();
        if (next[len - 1] != -1 && len % (len - (next[len - 1] + 1)) == 0){
            return true;
        }
        return false;

    }
};
```

```C++
//move mate solution
class Solution {
public:
    bool repeatedSubstringPattern(string s) {
        string t = s + s;
        t.erase(t.begin()); t.erase(t.end() - 1); // 掐头去尾
        if (t.find(s) != std::string::npos) return true; // r
        return false;
    }
};
```

