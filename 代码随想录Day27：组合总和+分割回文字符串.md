# [39. 组合总和 - 力扣](https://leetcode.cn/problems/combination-sum/description/)

## 题解思路

**回溯三部曲**

- 在面试的时候要考虑到0的情况，不能无限选取0，但是本题限制了不超过200的情况
- 递归函数参数：
  - 和组合问题一样，使用两个参数来保存符合条件的结果；
  - 什么时候需要使用startIndex来控制循环起始位置；
    - 在一个集合当中选取组合的话，就需要使用startIndex来控制起始位置
    - 如果是在多个不同集合当中选取，且集合互不影响，则不需要startIndex
- 递归终止条件：
  - 当sum大于target的时候终止
  - 当sum等于target的时候收集结果终止
- 单层搜索逻辑：
  - 循环从startIndex开始，搜索candidate集合
  - 这里使用startIndex的效果是不要搜索已经搜索过的位置
  - cabdidate[0], candidate[1] 和 candidate[1], candidate[0]的效果是一样的所以需要startIndex来标识

## 示例代码

```C++
class Solution {
public:
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(vector<int>& candidates, int target, int sum, int startIndex){
        if (sum > target){return;}
        if (sum == target){
            result.push_back(path);
            return ;
        }
        for (int i = startIndex; i < candidates.size(); i++){
            sum += candidates[i];
            path.push_back(candidates[i]);
            backtracking(candidates, target, sum, i);
            sum -= candidates[i];
            path.pop_back();
        }
    }
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        result.clear();
        path.clear();
        backtracking(candidates, target, 0, 0);
        return result;
    }
};
```

# [40. 组合总和 ](https://leetcode.cn/problems/combination-sum-ii/description/)

## 题解思路

**回溯三部曲**

- 确定回溯参数：使用一个used数组来记录统一树枝上的元素是否使用过
- 递归终止条件：当求和大于target时终止，当符合条件时收集结果
- 单层递归逻辑：如果前后两个候选元素相等，并且前一个元素的标记为false，说明前一个树枝使用过了这个元素，for循环当中进行continue对当前树层进行筛选

## 示例代码

```C++
class Solution {
public:
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(vector<int>& candidates, int target, int sum, int startIndex, vector<bool>& used){
        if (sum > target){
            return;
        }
        if (sum == target){
            result.push_back(path);
            return;
        }
        for (int i = startIndex; i < candidates.size(); i++){
            if (i > 0 && candidates[i] == candidates[i - 1] && used[i - 1] == false){
                continue;
            }
            sum += candidates[i];
            path.push_back(candidates[i]);
            used[i] = true;
            backtracking(candidates, target, sum, i + 1, used);
            used[i] = false;
            sum -= candidates[i];
            path.pop_back();
        }
    }
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        vector<bool> used(candidates.size(), false);
        path.clear();
        result.clear();
        sort(candidates.begin(), candidates.end());
        backtracking(candidates, target, 0, 0, used);
        return result;

    }
};
```



# [131. 分割回文串 ](https://leetcode.cn/problems/palindrome-partitioning/)

## 题解思路

**1.如何切割字符串**

- 切割问题是从字符串当中选出一段子串，可以用组合思维来思考：将子串组合起来
- 使用SubStr切割字符串

**2.回溯三部曲**

- 确定递归参数：递归参数还是字符串和起始位置
- 确定终止条件：当切割位置到达末尾时终止
- 单层递归逻辑：从startIndex开始到i的位置都是切割位置，判断这个字符串是否是回文字符串，是则将这个切割结果放入path中，不是则继续向下递归i+1

## 示例代码

```C++
class Solution {
public:
    vector<vector<string>> result;
    vector<string> path;
    void backtracking(string& s, int startIndex){
        if (startIndex >= s.size()){
            result.push_back(path);
            return;
        }
        for (int i = startIndex; i < s.size(); i++){
            if (isPalindrome(s, startIndex, i)){
                string str = s.substr(startIndex, i - startIndex + 1);
                path.push_back(str);
            }else{
                continue;
            }
            backtracking(s, i + 1);
            path.pop_back();
        }
    }
    bool isPalindrome(string& s, int left, int right){
        for (int i = left, j = right; i < j; i++, j--){
            if (s[i] != s[j]){return false;}
        }
        return true;
    }
    vector<vector<string>> partition(string s) {
        result.clear();
        path.clear();
        backtracking(s, 0);
        return result;
    }
};
```

