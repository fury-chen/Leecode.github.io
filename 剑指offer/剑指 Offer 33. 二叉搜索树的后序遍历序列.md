# [LCR 152. 验证二叉搜索树的后序遍历序列](https://leetcode.cn/problems/er-cha-sou-suo-shu-de-hou-xu-bian-li-xu-lie-lcof/description/)

## 题解思路（K神）

**递归分治**

- 根据BST的定义，通过递归，判断所有子树的正确性，如果所有子树都正确，则此序列为二叉搜索树的后序遍历
- 性质：后序遍历左子树都小于根结点，右子树都大于根结点，按照大小突变的位置划分左右子树
- 递归分析：
  - 终止条件：当i >= j时表明此区间<=1无需判断，返回TRUE
  - 递推：划分左右子树，遍历后序遍历的[i, j]区间，找第一个比根结点大的结点，索引标记为m，划分出左区间为[i, m - 1]、右子树区间[m, j -  1]根结点索引为j
  - 判断：左子树区间[i, m - 1]内所有结点都应该小于当前根结点j，而划分左右子树时已经保证了左子树区间正确性，因此只需要判断右子树区间即可；右子树区间的[m, j -1]区间元素都应该大于j，实现方式为遍历，当遇到小于等于的结点就跳出，当遍历到末尾则证明TRUE

## 示例代码

```C++
class Solution {
public:
    bool recursion(vector<int> postorder, int left, int right){
        if (left >= right) return true;
        int index = left;
        while (postorder[index] < postorder[right]) index++;
        int boundary = index;
        while (postorder[index] > postorder[right]) index++;
        return index == right && recursion(postorder, left, boundary - 1) && recursion(postorder, boundary, right - 1);
    }
    bool verifyTreeOrder(vector<int>& postorder) {
        return recursion(postorder, 0, postorder.size() - 1);
    }
};
```

