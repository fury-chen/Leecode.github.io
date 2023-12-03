# [剑指offer 03. 数组中重复的数字](https://www.acwing.com/problem/content/14/)

```C++
class Solution {
public:
    int duplicateInArray(vector<int>& nums) {
        if (nums.size() > 1000 || nums.size() <= 0) return -1;
        int arr[1001] = {0};
        for (int i = 0; i < nums.size(); i++){
            if (nums[i] < 0 || nums[i] > nums.size()) return -1;
            arr[nums[i]]++;
        }
        for (int i = 0; i < 1001; i++){
            if (arr[i] > 1) return i;
        }
        return -1;
    }
};
```

- 非常傻逼的加了一些条件，总而言之还是使用Hash法做映射，和字母异位词类似

  
