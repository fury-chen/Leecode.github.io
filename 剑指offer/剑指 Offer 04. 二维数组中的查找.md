

# [剑指 Offer 04. 二维数组中的查找](https://www.acwing.com/problem/content/description/16/)

```C++
class Solution {
public:
    bool searchArray(vector<vector<int>> array, int target) {
        if (array.empty() || array[0].empty()) return false;
        int length = array.size() - 1;
        //std::cout << length << std::endl;
        int i = length;
        int j = 0;
        while (i >= 0 && j <= length){
            if (array[i][j] > target) i--;
            else if (array[i][j] < target) j++;
            else return true;
        }
        return false;
    }
};
```

- 注意array.empty()的使用，卡在了一个空集上

- 思路是先左后右，先逐行查找，随后逐列查找

- 获取length的方法如上所示

  
