

# [剑指 Offer 11. 旋转数组的最小数字](https://www.acwing.com/problem/content/20/)

```C++
class Solution {
public:
    int findMin(vector<int>& nums) {
        if (nums.size() == 0) return -1;
        int left = 0;
        int right = nums.size() - 1;
        while (right > left){
            int mid = left + (right - left) / 2;
            if (nums[right] > nums[left]){return nums[left];}
            if (nums[mid] < nums[right]){right = mid;}
            else if (nums[mid] > nums[right]){left = mid + 1;}
            else {right--;}
        }
        return nums[left];
    }
};
```

