# [738. 单调递增的数字](https://leetcode.cn/problems/monotone-increasing-digits/description/)

## 题解思路

- 从后往前遍历的好处：能够利用后面的信息保持单调递增的关系
- flag的作用，标记从哪一位开始往后将其全都处理成‘9'，有可能用不到flag的逻辑
- 当非单调递增的时候将前一位-1
- 代码细节很多，有不少反常识的编程逻辑



## 示例代码

```C++
class Solution {
public:
    int monotoneIncreasingDigits(int n) {
        string strNum = to_string(n);
        int flag = strNum.size();
        for (int i = strNum.size() - 1; i > 0; i--){
            if (strNum[i - 1] > strNum[i]){
                flag = i;
                strNum[i - 1]--;
            }
        }
        for (int i = flag; i < strNum.size(); i++){
            strNum[i] = '9';
        }
        return stoi(strNum);
    }
};
```

# [968. 监控二叉树 ](https://leetcode.cn/problems/binary-tree-cameras/)

## 题解思路

**贪心思路**

- 优先考虑叶子结点（叶子结点多+后序）：在叶子结点的父节点上层放一个摄像头能够尽可能覆盖多的范围
- 从后序顺序遍历时，每隔两个节点就放一个摄像头，遍历到空节点的时候只能将其状态设置为有覆盖
- 摄像头状态（3）：有无覆盖（state 0 2），有摄像头（state1）
- 四类情况：
  - 左右节点都有覆盖，此时中间节点应该是无覆盖的状态<img src="https://code-thinking-1253855093.file.myqcloud.com/pics/20201229203710729.png" alt="968.监控二叉树2" style="zoom:33%;" />
  - 左右节点至少有一个无覆盖的情况：摄像头的数量+1，且return state1
    - left == 0 && right == 0 左右节点无覆盖
    - left == 1 && right == 0 左节点有摄像头，右节点无覆盖
    - left == 0 && right == 1 左节点有无覆盖，右节点摄像头
    - left == 0 && right == 2 左节点无覆盖，右节点覆盖
    - left == 2 && right == 0 左节点覆盖，右节点无覆盖
  - 左右节点至少有一个有摄像头
  - 头结点无覆盖

## 示例代码

```C++
class Solution {
private:
    int result;
    int traversal(TreeNode* cur) {
        if (cur == NULL) return 2;
        int left = traversal(cur->left);    // 左
        int right = traversal(cur->right);  // 右
        if (left == 2 && right == 2) return 0;
        else if (left == 0 || right == 0) {
            result++;
            return 1;
        } else return 2;
    }
public:
    int minCameraCover(TreeNode* root) {
        result = 0;
        if (traversal(root) == 0) { // root 无覆盖
            result++;
        }
        return result;
    }
};
```

