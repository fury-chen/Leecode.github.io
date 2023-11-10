# [332. 重新安排行程](https://leetcode.cn/problems/reconstruct-itinerary/)

## 题解思路

**记录映射关系**

- 如果让多个机场之间要有顺序就需要使用std::map或者std::multiset来进行映射机场之间的飞行关系
- 选择map结构来进行对应，将其设置为unordered_map<off_Plane, map<land_Plane, times>> target可以使用航班次数times来进行增减，标记机场是否已经使用过了
- 如果航班次数>0则说明目的地还能飞，如果航班次数=0说明目的地不能飞了，不用对其集合做删除或增加元素的操作

**回溯法**

- 递归参数：飞行次数，行程安排结果
- 终止条件：遇到机场个数满足总机场数
- 单层搜索逻辑：回溯模板

## 示例代码

```C++
class Solution {
private:
    unordered_map<string, map<string, int>> targets;//映射关系记录：（起飞机场，（降落机场，航班次数））
    bool backtracking(int ticketNum, vector<string>& result){
        if (result.size() == ticketNum + 1){
            //终止条件为所有机票都使用过了
            return true;
        }
        for (pair<const string, int>& target : targets[result[result.size() - 1]]){
            if (target.second > 0){
                result.push_back(target.first);
                target.second--;
                if (backtracking(ticketNum, result)) return true;
                result.pop_back();
                target.second++;
            }
        }
        return false;
    }
public:
    vector<string> findItinerary(vector<vector<string>>& tickets) {
        targets.clear();
        vector<string> result;
        for (const vector<string>& vec : tickets){
            targets[vec[0]][vec[1]]++;//[vec[0]][vec[1]]= 航班次数
        }
        result.push_back("JFK");
        backtracking(tickets.size(), result);
        return result;
    }
};
```

# [51. N 皇后](https://leetcode.cn/problems/n-queens/)

## 示例代码

**回溯三部曲**

- 通过遍历n行n列的棋盘来进行回溯，每次摆放都判断一次是否符合条件，不符合就pop
- 由此思路：模拟棋盘+合法判断+回溯模板
- 难点在于如何进行合法判断，代码的操作能力

**复杂度分析**

- 时间复杂度：O(n！)
- 空间复杂度：O(n)

## 题解思路

```C++
class Solution {
private:
    vector<vector<string>> result;
    void backtracking(int n, int row, vector<string>& chessboard){
        if (row == n){
            result.push_back(chessboard);
            return ;
        }
        for (int col = 0; col < n; col++){
            if (isValid(row, col, chessboard, n)){
                chessboard[row][col] = 'Q';
                backtracking(n, row + 1, chessboard);
                chessboard[row][col] = '.';}
        }
    }
    bool isValid(int row, int col, vector<string>& chessboard, int n){
        for (int i = 0; i < row; i++){
            if (chessboard[i][col] == 'Q'){
                return false;
            }
        }
        for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--){
            if (chessboard[i][j] == 'Q'){
                return false;
            }
        }
        for (int i = row - 1, j = col + 1; i >= 0 && j < n; i--, j++){
            if (chessboard[i][j] == 'Q'){
                return false;
            }
        }
        return true;
    }
public:
    vector<vector<string>> solveNQueens(int n) {
        result.clear();
        std::vector<std::string> chessboard(n, std::string(n,'.'));
        backtracking(n, 0, chessboard);
        return result;
    }
};
```

# [37. 解数独 ](https://leetcode.cn/problems/sudoku-solver/)

## 题解思路

**回溯法**

- 递归函数返回值：bool——只要找到一个满足的棋盘就返回（单个结果）
-  棋盘问题求解：
  - 行是否重复
  - 列是否重复
  - 九宫格里是否重复

## 示例代码

```C++
class Solution {
private:
bool backtracking(vector<vector<char>>& board) {
    for (int i = 0; i < board.size(); i++) {        // 遍历行
        for (int j = 0; j < board[0].size(); j++) { // 遍历列
            if (board[i][j] == '.') {
                for (char k = '1'; k <= '9'; k++) {     // (i, j) 这个位置放k是否合适
                    if (isValid(i, j, k, board)) {
                        board[i][j] = k;                // 放置k
                        if (backtracking(board)) return true; // 如果找到合适一组立刻返回
                        board[i][j] = '.';              // 回溯，撤销k
                    }
                }
                return false;  // 9个数都试完了，都不行，那么就返回false
            }
        }
    }
    return true; // 遍历完没有返回false，说明找到了合适棋盘位置了
}
bool isValid(int row, int col, char val, vector<vector<char>>& board) {
    for (int i = 0; i < 9; i++) { // 判断行里是否重复
        if (board[row][i] == val) {
            return false;
        }
    }
    for (int j = 0; j < 9; j++) { // 判断列里是否重复
        if (board[j][col] == val) {
            return false;
        }
    }
    int startRow = (row / 3) * 3;
    int startCol = (col / 3) * 3;
    for (int i = startRow; i < startRow + 3; i++) { // 判断9方格里是否重复
        for (int j = startCol; j < startCol + 3; j++) {
            if (board[i][j] == val ) {
                return false;
            }
        }
    }
    return true;
}
public:
    void solveSudoku(vector<vector<char>>& board) {
        backtracking(board);
    }
};

```

