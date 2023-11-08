# [491. 递增子序列 ](https://leetcode.cn/problems/non-decreasing-subsequences/description/)

## 题解思路

- 本题回溯去重思路类似子集当中的树层去重，但是不能修改原本数组的顺序，因此需要用Hash法进行去重查找
- 递归函数参数：组合思路
- 终止条件：每次path放新的元素进来就push_back到result中
- 单层搜索逻辑：判断是否递增，使用set进行查重

## 示例代码

```C++
class Solution {
public:
    vector<int> path;
    vector<vector<int>> result;
    void backtracking(vector<int>& nums, int startIndex){
        if (path.size() > 1){
            result.push_back(path);
        }
        unordered_set<int> uset;
        for (int i = startIndex; i < nums.size(); i++){
            if ((!path.empty() && nums[i] < path.back()) || uset.find(nums[i]) != uset.end()){
                continue;
            }
            uset.insert(nums[i]);
            path.push_back(nums[i]);
            backtracking(nums, i + 1);
            path.pop_back();
        }
    }
    vector<vector<int>> findSubsequences(vector<int>& nums) {
        result.clear();
        path.clear();
        backtracking(nums, 0);
        return result;
    }
};
```

# [46. 全排列 ](https://leetcode.cn/problems/permutations/)

## 题解思路：

**回溯三部曲**

- 全排列去重：使用used数组来进行查重
- 每层都是从0开始搜索，不需要startIndex

**复杂度分析**

- 时间复杂度：O(n!)
- 空间复杂度：O(n)

## 示例代码

```C++
class Solution {
public:
    vector<int> path;
    vector<vector<int>> result;
    void backtracking(vector<int> nums, vector<bool> used){
        if (path.size() == nums.size()){
            result.push_back(path);
            return;
        }
        for (int i = 0; i < nums.size(); i++){
            if (used[i] == true) continue;
            path.push_back(nums[i]);
            used[i] = true;
            backtracking(nums, used);
            used[i] = false;
            path.pop_back();
        }
        
    }
    vector<vector<int>> permute(vector<int>& nums) {
        result.clear();
        path.clear();
        vector<bool> used(nums.size(), false);
        backtracking(nums, used);
        return result;
    }
};
```

# [47. 全排列 II ](https://leetcode.cn/problems/permutations-ii/description/)

## 题解思路

- 可以修改原数组顺序，将其排序通过used进行去重，每次都要从0开始遍历，当遇到used=true则跳过
- 回溯三部曲

## 示例代码

```C++
class Solution {
public:
    vector<int> path;
    vector<vector<int>> result;
    void backtracking(vector<int> nums, vector<bool> used){
        if (path.size() == nums.size()){
            result.push_back(path);
            return;
        }
        for (int i = 0; i < nums.size(); i++){
            if (i > 0 && nums[i] == nums[i - 1] && used[i - 1] == false){
                continue;
            }
            if (used[i] == false){
                used[i] = true;
                path.push_back(nums[i]);

                backtracking(nums, used);
                path.pop_back();
                used[i] = false;
            }
        }
    }
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        result.clear();
        path.clear();
        sort(nums.begin(), nums.end());
        vector<bool> used(nums.size(), false);
        backtracking(nums, used);
        return result;
    }
};
```

