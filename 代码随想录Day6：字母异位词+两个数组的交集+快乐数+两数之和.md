# [242.有效的字母异位词](https://leetcode.cn/problems/valid-anagram/)

## 题解思路

**1.数组哈希**

- 字母构建哈希关系：使用一个大小为26的数组来表示字母的哈希映射关系
- 异位词判断：对数组1和数组2进行+-操作，如果最后每个字母的数量都为零，则异位词

**2.复杂度分析**

- 时间复杂度：O(n)
- 空间复杂度：O(1)

## 示例代码

```C++
class Solution {
public:
    bool isAnagram(string s, string t) {
        int record[26] = 0;
        for (int i = 0; i < s.size(); i++){
            record[s[i] - 'a']++;
        }
        for (int j = 0; j < t.size(); j++){
            record[t[j] - 'a']--;
        }
        for (int k = 0; k < 26; k++){
            if (record[k] != 0){
                return false
            }
        }
        return true;
    }
};
```

# [349.两个数组的交集](https://leetcode.cn/problems/intersection-of-two-arrays/)

## 题解思路

**1.Hash查找**

- 选择Hash结构：如果Hash值较少、特别分散、跨度很大，使用数组就会造成很大浪费，因此可以选用set或map，看具体问题中是否需要统计value
- 三种set数据结构：
  - std::set —— 红黑树底层
  - std::multiset——红黑树底层
  - shd::unordered_set——Hash表底层

**2.复杂度分析**

- 时间复杂度：O(mn)
- 空间复杂度：O(n)

## 示例代码

```C+
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        unordered_set<int> result_set;
        unordered_set<int> num_set(nums1.begin(), nums1.end());
        for (int num : nums2){//这个写的好像Python
            if (num_set.find(num) != num_set.end()){
                result_set.insert(num);
            }
        }
        return vector<int>(result_set.begin(), result_set.end());
    }
};
```

# [202.快乐数](https://leetcode.cn/problems/happy-number/)

## 题解思路

**1.快乐数判断**

- 无限循环的原因是重复出现相同的数字，数字之间形成循环，因此终止循环的条件是满足快乐数条件或者出现重复数字
- 使用一个unordered_set来查找数字是否出现过
- 取数字每一位的平方（取n的每一位数字）
- Hash查找：uset.find(key) != uset.end()  1表示找到，0表示没有
- 总结：Hash结构用来查找一个变量是否已经出现过；按位取数操作

**2.复杂度分析**

- 时间复杂度：O(logn)
  - 首先getSum的时间复杂度为O(logn)，因为在while循环中，每次循环都将n除以10，直到变为0
  - 其次isHappy函数的时间复杂度取决于getSum函数的时间复杂度，在while循环中，每次循环都调用getSum，所以整体为O(logn)
- 空间复杂度：O(logn)

**3.unordered_set容器**

-  不再以键值对形式存储数据，而是直接存储数据的值
-  容器内部存储的各个元素的值都互不相等，且不能被修改
-  不会对内部存储的数据进行排序
-  成员方法find说明：uset.find(key)查找以值为key 的元素，如果找到，则返回一个指向该元素的正向迭代器；反之返回一个指向容器最后一个元素之后位置的迭代器（即end()方法）

## 示例代码

```C++
class Solution {
public:
    int getSum(int n){
        int sum = 0;
        while (n){
            sum += (n % 10) * (n % 10);
            n /= 10;
        }
        return sum;
    }
    bool isHappy(int n) {
        unordered_set<int> sum_set;
        while (1)
        {
            int sum = getSum(n);
            if (sum == 1){
                return true;
            }
            if (sum_set.find(sum) != sum_set.end()){
                return false;
            }
            else {sum_set.insert(sum);}
            n = sum;
        }
        
    }
};
```

# [1.两数之和](https://leetcode.cn/problems/two-sum/)

## 题解思路

**1.哈希查询**

- 本题一开始尝试使用uset去解决，但是发现if (uset.find(target -  nums[i]) != uset.end())存不了下标信息
- map结构：pair<key, value>结构，使用key保存数值，使用value保存下标

**2.复杂度分析**

- 时间复杂度：O(n)
- 空间复杂度：O(1）

## 示例代码

```C++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        std::unordered_map<int, int> umap;
        for (int i = 0; i < nums.size(); i++){
            auto iter = umap.find(target - nums[i]);
            if (iter != umap.end()){
                return {iter->second, i};
            }
            umap.insert(pair<int, int>(nums[i], i));
        }
        return {};
    }
};
```



