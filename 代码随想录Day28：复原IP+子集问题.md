# [93. 复原 IP 地址 ](https://leetcode.cn/problems/restore-ip-addresses/description/)

## 题解思路
**IP的判断**
- 逐个字符串往右去遍历，判断数字大小
- 首字符不能为0
- 百位字符不能超过255
  
**回溯法**
- 判断这串数字是否合法，如果合法则在字符尾部+'.'
- 回溯时要删除'.'
- 记录.的数量，当到达三个时证明为一个合法IP，判断最后的一段是否符合要求，收集结果

## 示例代码

```c++
class Solution {
public:
    vector<string> result;
    bool isValid(const string& s, int start, int end){
        if (start > end){
            return false;
        }
        if (s[start] == '0' && start != end){
            return false;
        }
        int num = 0;
        for (int i = start; i <= end; i++){
            if (s[i] > '9' || s[i] < '0'){
                return false;
            }
            num = num * 10 + (s[i] - '0');
            if (num > 255){
                return false;
            }
        }
        return true;
    }
    void backtracking(string& s, int startIndex, int pointNum){
        if (pointNum == 3){
            if (isValid(s, startIndex, s.size() - 1)){
                result.push_back(s);
            }
            return;
        }
        for (int i = startIndex; i < s.size(); i++){
            if (isValid(s, startIndex, i)){
                s.insert(s.begin() + i + 1, '.');
                pointNum++;
                backtracking(s, i + 2, pointNum);
                pointNum--;
                s.erase(s.begin() + i + 1);
            }else break;
        }
    }

    vector<string> restoreIpAddresses(string s) {
        result.clear();
        if (s.size() < 4 || s.size() > 12){
            return result;
        }
        backtracking(s, 0, 0);
        return result;
    }
};
```



# [78. 子集 ](https://leetcode.cn/problems/subsets/description/)

## 题解思路

- 本题回溯的思路和组合问题一致，区别点在于本题是收集所有节点

## 示例代码

```C++
class Solution {
public:
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(vector<int>& nums, int startIndex){
        if (startIndex > nums.size()) return;
        if (startIndex <= nums.size()){
            result.push_back(path);
        }
        for (int i = startIndex; i < nums.size(); i++){
            path.push_back(nums[i]);
            backtracking(nums, i + 1);
            path.pop_back();
        }
    }
    vector<vector<int>> subsets(vector<int>& nums) {
        path.clear();
        result.clear();
        backtracking(nums, 0);
        return result;
    }
};
```



# [90. 子集 II](https://leetcode.cn/problems/subsets-ii/)

## 题解思路
- 通过排序和used数组来进行去重
- 这个题的去重是不能有重复集合，即树层内进行去重

## 示例代码

```c++
class Solution {
public:
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(vector<int>& nums, int startIndex, vector<bool>& used){
        result.push_back(path);
        if (startIndex >= nums.size()) return;
        for (int i = startIndex; i < nums.size(); i++){
            if (i > 0&& nums[i] == nums[i - 1] && used[i - 1] == false){continue;}
            else{
                path.push_back(nums[i]);
                used[i] = true;
                backtracking(nums, i + 1, used);
                path.pop_back();
                used[i] = false;
            }
        }
    }
    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        if (nums.size() == 0) return result;
        sort(nums.begin(), nums.end());
        vector<bool> used(nums.size(), false);
        backtracking(nums, 0, used);
        return result;
    }
};

```

