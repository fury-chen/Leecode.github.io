# 链表题单整理Part2（参考灵神题单）

本帖先为灵神打call，从代码随想录过渡到灵神的题单来应对笔试：

[分享丨【题单】链表、二叉树与一般树（前后指针/快慢指针/DFS/BFS/直径/LCA）](https://leetcode.cn/circle/discuss/K0n2gO/)



## 链表操作：

### 遍历链表：

- [1290. 二进制链表转整数 - 力扣（LeetCode）](https://leetcode.cn/problems/convert-binary-number-in-a-linked-list-to-integer/)

  ```C++
  class Solution {
  public:
      int getDecimalValue(ListNode* head) {
          ListNode* cur = head;
          int res = 0;
          while (cur){
              res = cur->val + res * 2;
              cur = cur->next;
          }
          return res;
      }
  };
  ```

  

- [2058. 找出临界点之间的最小和最大距离 - 力扣（LeetCode）](https://leetcode.cn/problems/find-the-minimum-and-maximum-number-of-nodes-between-critical-points/description/)

  ```C++
  class Solution {
  public:
      vector<int> nodesBetweenCriticalPoints(ListNode* head) {
          vector<int> node;
          ListNode* cur = head;
          while (cur){
              node.push_back(cur->val);
              cur = cur->next;
          }
          vector<int> position;
          for (int i = 1; i < node.size() - 1; i++){
              if (node[i] > node[i - 1] && node[i] > node[i + 1] || node[i] < node[i - 1] && node[i] < node[i + 1]) position.push_back(i);
          }
          if (position.size() < 2) return {-1, -1};
          int minDistance = INT_MAX, maxDistance = INT_MIN;
          for (int i = 1; i < position.size(); i++){
              minDistance = min(position[i] - position[i - 1], minDistance);
          }
          maxDistance = position.back() - position[0];
          return {minDistance, maxDistance};
      }
  };
  
  //写法2
  class Solution {
  public:
      vector<int> nodesBetweenCriticalPoints(ListNode* head) {
          ListNode* prev = head;
          ListNode* cur = head->next;
          ListNode* after = head->next->next;
          vector<int> position;
          int index = 1;
          while (after){
              if (cur->val < prev->val && cur->val < after->val || cur->val > prev->val && cur->val > after->val) 
                  position.push_back(index);
              prev = prev->next;
              cur = cur->next;
              after = after->next;
              index++;
          }
          if (position.size() < 2) return {-1, -1};
          int minDistance = INT_MAX, maxDistance = position.back() - position[0];
          for (int i = 1; i < position.size(); i++){
              minDistance = min(minDistance, position[i] - position[i - 1]);
          }
          return {minDistance, maxDistance};
      }
  };
  ```

- [2181. 合并零之间的节点 - 力扣（LeetCode）](https://leetcode.cn/problems/merge-nodes-in-between-zeros/description/)

  ```C++
  class Solution {
  public:
      ListNode* mergeNodes(ListNode* head) {
          ListNode* dummyHead = new ListNode();
          ListNode* tail = dummyHead;
          int sumVal = 0;
          for (ListNode* cur = head->next; cur; cur = cur->next){
              if (cur->val == 0){
                  ListNode* node = new ListNode(sumVal);
                  tail->next = node;
                  tail = tail->next;
                  sumVal = 0;
              } else {
                  sumVal += cur->val;
              }
          }
          return dummyHead->next;
      }
  };
  ```

- [725. 分隔链表 - 力扣（LeetCode）](https://leetcode.cn/problems/split-linked-list-in-parts/description/)

  ```C++
  class Solution {
  public:
      vector<ListNode*> splitListToParts(ListNode* head, int k) {
          ListNode* cur = head;
          int len = 0;
          while (cur){
              len++;
              cur = cur->next;
          }
          int quotient = len / k;
          int remainder = len % k;
          cur = head;
          vector<ListNode*> ans(k, nullptr);
          for (int i = 0; i < k && cur; i++){
              ans[i] = cur;
              int partSize = quotient + (i < remainder ? 1 : 0);
              for (int j = 1; j < partSize; j++){
                  cur = cur->next;
              }
              ListNode* nextNode = cur->next;
              cur->next = nullptr;
              cur = nextNode;
          }
          return ans;
      }
  };
  ```

  

- [817. 链表组件 - 力扣（LeetCode）](https://leetcode.cn/problems/linked-list-components/description/)

  ```C++
  //本题需要计算组件的个数=链表当中计算有多少组件的起始位置即可，当节点满足下列条件之一时，是组件的起始位置：
  //1.节点的值在nums中且节点位于链表的起始位置；
  //2.节点的值在nums中且节点的前一个节点不在nums中；
  class Solution {
  public:
      int numComponents(ListNode* head, vector<int>& nums) {
          unordered_set<int> numSet;
          for (int num : nums){
              numSet.emplace(num);
          }
          //或者直接初始化：unordered_set<int> numSet(nums.begin(), nums.end());
          bool inSet = false;
          int res = 0;
          ListNode* cur = head;
          while (cur){
              if (numSet.count(cur->val)){
                  if (!inSet) {
                      inSet = true;
                      res++;
                  }
              } else {
                  inSet = false;
              }
              cur = cur->next;
          }
          return res;
      }
  };
  ```

  
