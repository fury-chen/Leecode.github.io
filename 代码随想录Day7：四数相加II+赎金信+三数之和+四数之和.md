# [454.四数相加](https://leetcode.cn/problems/4sum-ii/)

## 题解思路

**1.解题步骤**

> 简单的一个哈希查重并统计符合条件数目，对map.find()方法的运用

- 使用unordered_map来存放集合1和集合2的两数之和
- 继续查找集合34来判断和是否为0
- 为0则计数，反之则继续遍历

**2.复杂度分析**

- 时间复杂度：O(n^2）
- 空间复杂度：O(n^2)

## 示例代码

```c++
class Solution {
public:
    int fourSumCount(vector<int>& nums1, vector<int>& nums2, vector<int>& nums3, vector<int>& nums4) {
        unordered_map<int, int> numMap;
        for (int i = 0; i < nums1.size(); i++){
            for (int j = 0; j < nums2.size(); j++){
                numMap[nums1[i] + nums2[j]]++;
            }
        }

        int count = 0;
        for (int i = 0; i < nums3.size(); i++){
            for (int j = 0; j < nums4.size(); j++){
                if (numMap.find(0 - (nums3[i] + nums4[j])) != numMap.end()){
                    count += numMap[0 - (nums3[i] + nums4[j])];
                }
            }
        }
        return count;
    }
};
```



# [383. 赎金信 ](https://leetcode.cn/problems/ransom-note/)

## 题解思路

**1.解题要点**

- 使用哈希法记录赎金信当中的字符数目，通过+-方法来统计两两之间数量是否一致
- 可以使用map或array，但是使用array维护起来更省时间，map思路也很简单

**2.错误梳理**

- 一开始写了三个循环，后面看Carl写的发现可以优化
- 数组方法的思路上先记录magazine，再往下减，减完如果不足则false
- 在写if判断的时候遗漏了-'a'导致找的位置不对

**3.复杂度分析**

- 时间复杂度：O(n)
- 空间复杂度：O(1)

## 示例代码

```C++
//unordered_map Solution
class Solution {
public:
    bool canConstruct(string ransomNote, string magazine) {
        unordered_map<char, int> umap;
        for (int i = 0; i < ransomNote.size(); i++){
            umap[ransomNote[i]]++;
        }
        for (int j = 0; j < magazine.size(); j++){
            umap[magazine[j]]--;
        }
        for (auto it = umap.begin(); it != umap.end(); it++){
            if (it->second > 0){
                return false;
            }
        }
        return true;
    }
};
```

```C++
//array Solution
class Solution {
public:
    bool canConstruct(string ransomNote, string magazine) {
        int recode[26] = {0};
        for (int i = 0; i < magazine.length(); i++){
            recode[magazine[i] - 'a']++;
        }
        for (int j = 0; j < ransomNote.length(); j++){
            recode[ransomNote[j] - 'a']--;
            if (recode[ransomNote[j] - 'a'] < 0) return false;
        }
        return true;
    }
};
```



# [15. 3Sum ](https://leetcode.cn/problems/3sum/)

## 题解思路
**1.哈希解法**
- 两重循环嵌套求和，第三数通过0 - (nums[i] + nums[j])来查找控制
- 要对元组的三个元素分别进行去重，但又要保持不同元素和为0的答案，即答案位置是特殊的，但元素位置不是特殊的
- 排序过后的数组容易去重，因为大小相同的元素都相邻
- 去重剪枝的方法是判断同一个答案位置是否选到了两个相同的元素，即每次挑选之前先将后1，2，1个重复元素去掉
- 如果排序之后第一个数就>0则不可能和为0
- 
**2.复杂度分析**
- 时间复杂度：O(n^2)
- 空间复杂度：O(n）
- 
**3.双指针法**
- 先排序后去重，有序数组三数和可以通过移动左右两个指针控制与0的关系
- 去重和找到一组满足条件的元组都要将左右指针向内移动
- 
**4.复杂度分析**
- 时间复杂度：O(n^2)
- 空间复杂度：O(1)


## 示例代码

```C++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> result;
        sort(nums.begin(), nums.end());
        for (int i = 0; i < nums.size(); i++){
            if (nums[i] > 0){
                break;
            }
            if (i > 0 && nums[i] == nums[i - 1]){
                continue;
            }
            unordered_set<int> set;
            for (int j = i + 1; j < nums.size(); j++){
                if (j > i + 2 && nums[j] == nums[j-1] && nums[j-1] == nums[j-2]) { 
                    continue;
                }
                int c = 0 - (nums[i] + nums[j]);
                if (set.find(c) != set.end()){
                    result.push_back({nums[i], nums[j], c});
                    set.erase(c);
                } else {
                    set.insert(nums[j]);
                }
            }
        }
        return result;
    }
};
//超级麻烦的剪枝
```

```C++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> result;
        sort(nums.begin(), nums.end());
        for (int i = 0; i < nums.size(); i++){
            if (nums[i] > 0){
                break;
            }
            if (i > 0 && nums[i] == nums[i - 1]){
                continue;
            }
            int left = i + 1;
            int right = nums.size() - 1;
            while (right > left){
                if (nums[i] + nums[left] + nums[right] > 0){
                    right--;
                }
                else if (nums[i] + nums[left] + nums[right] < 0){
                    left++;
                }
                else{
                    result.push_back(vector<int>{nums[i], nums[left], nums[right]});
                    while(right > left && nums[right] == nums[right - 1]) {right--;}
                    while(right > left && nums[left] == nums[left + 1]){left++;}
                    left++;
                    right--; 
                }
            }
        }
        return result;
        
    }
};
```

# [18.四数之和](https://leetcode.cn/problems/4sum/)
## 题解思路
**1.双指针法**
- target不是一个具体的数，可能正数可能负数
- 四个数=两层循环+双指针
- 剪枝操作包括：第一个数的大小剪枝、每个位置的数重复剪枝


**2.复杂度分析**
- 时间复杂度：O(n^3)
- 空间复杂度：O(1)
  
## 示例代码
```C++
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        vector<vector<int>> result;
        sort(nums.begin(), nums.end());
        for (int i = 0; i < nums.size(); i++){
            if (nums[i] > target && nums[i] >= 0){
                break;
            }
            if (i > 0 && nums[i] == nums[i - 1]){
                continue;
            }
            for (int j = i + 1; j < nums.size(); j++){
                if (nums[i] + nums[j] > target && nums[i] + nums[j] >= 0){
                    break;
                }
                if (j > i + 1 && nums[j] == nums[j - 1]){
                    continue;
                }
                int left = j + 1;
                int right = nums.size() - 1;
                while (right > left){
                    if ((long) nums[i] + nums[j] + nums[left] + nums[right] > target){
                        right--;
                    }
                    else if ((long) nums[i] + nums[j] + nums[left] + nums[right] < target){
                        left++;
                    }
                    else{
                        result.push_back(vector<int>{nums[i], nums[j], nums[left], nums[right]});
                        while (right > left && nums[right] == nums[right - 1]) right--;
                        while (right > left && nums[left] == nums[left + 1]) left++;
                        right--;
                        left++; 
                    }
                }
            }
        }
        return result;
    }
};
```
