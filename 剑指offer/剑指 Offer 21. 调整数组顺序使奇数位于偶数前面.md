

# [剑指 Offer 21. 调整数组顺序使奇数位于偶数前面](https://www.acwing.com/problem/content/30/)

```C++
class Solution {
public:
    void reOrderArray(vector<int> &array) {
         int leftIndex = 0;
         int rightIndex = array.size() - 1;
         while (rightIndex > leftIndex){
             if (array[leftIndex] % 2 == 0 && array[rightIndex] % 2 == 1){
                 int tmp = array[leftIndex];
                 array[leftIndex] = array[rightIndex];
                 array[rightIndex] = tmp;
                 leftIndex++;
                 rightIndex--;
             }
            else if (array[leftIndex] % 2 == 1 && array[rightIndex] % 2 == 1){
                leftIndex++;
            }
            else if (array[leftIndex] % 2 == 0 && array[rightIndex] % 2 == 0){
                rightIndex--;
            }
            else {
                leftIndex++;
                rightIndex--;
            }
         }
    }
};
```

- 讨论四种情况（左右*奇偶）：分别对四种情况进行操作

- 进一步精简代码：

```C++
class Solution {
public:
    void reOrderArray(vector<int> &array) {
         int l = 0, r = array.size() - 1;
         while (l < r) {
             while (l < r && array[l] % 2 == 1) l ++ ;
             while (l < r && array[r] % 2 == 0) r -- ;
             if (l < r) swap(array[l], array[r]);
         }
    }
};

```

