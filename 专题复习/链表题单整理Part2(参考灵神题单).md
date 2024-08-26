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

  

### 删除节点

- [203. 移除链表元素 - 力扣（LeetCode）](https://leetcode.cn/problems/remove-linked-list-elements/description/)

  ```C++
  class Solution {
  public:
      ListNode* removeElements(ListNode* head, int val) {
          ListNode* dummyHead = new ListNode(0);
          dummyHead->next = head;
          ListNode* cur = dummyHead;
          while (cur->next){
              if (cur->next->val == val){
                  ListNode* tmp = cur->next;
                  cur->next = cur->next->next;
                  delete tmp;
                  tmp = nullptr;
              }
              else cur = cur->next;
          }
          return dummyHead->next;
      }
  };
  ```

- [3217. 从链表中移除在数组中存在的节点 - 力扣（LeetCode）](https://leetcode.cn/problems/delete-nodes-from-linked-list-present-in-array/description/)

  ```C++
  class Solution {
  public:
      ListNode* modifiedList(vector<int>& nums, ListNode* head) {
          unordered_set<int> numSet(nums.begin(), nums.end());
          ListNode* dummyHead = new ListNode(0);
          dummyHead->next = head;
          ListNode* cur = dummyHead;
          while (cur->next){
              if (numSet.count(cur->next->val)){
                  cur->next = cur->next->next;
              } else {
                  cur = cur->next;
              }
          }
          return dummyHead->next;
      }
  };
  ```

- [83. 删除排序链表中的重复元素 - 力扣（LeetCode）](https://leetcode.cn/problems/remove-duplicates-from-sorted-list/)

  ```C++
  class Solution {
  public:
      ListNode* deleteDuplicates(ListNode* head) {
          ListNode* cur = head;
          while (cur){
              while (cur->next && cur->val == cur->next->val){
                  cur->next = cur->next->next;
              }
              cur = cur->next;
          }
          return head;
      }
  };
  ```

- [82. 删除排序链表中的重复元素 II - 力扣（LeetCode）](https://leetcode.cn/problems/remove-duplicates-from-sorted-list-ii/description/)

  ```C++
  class Solution {
  public:
      ListNode* deleteDuplicates(ListNode* head) {
          ListNode* dummyHead = new ListNode(0);
          dummyHead->next = head;
          auto cur = dummyHead;
          while (cur->next && cur->next->next){
              int val = cur->next->val;
              if (cur->next->next->val == val){
                  while (cur->next && cur->next->val == val){
                      cur->next = cur->next->next;
                  }
              } else {
                  cur = cur->next;
              }
          }
          return dummyHead->next;
      }
  };
  ```

- [237. 删除链表中的节点 - 力扣（LeetCode）](https://leetcode.cn/problems/delete-node-in-a-linked-list/description/)

  ```C++
  class Solution {
  public:
      void deleteNode(ListNode* node) {
          node->val = node->next->val;
          node->next = node->next->next;
      }
  };
  ```

  

- [1669. 合并两个链表 - 力扣（LeetCode）](https://leetcode.cn/problems/merge-in-between-linked-lists/description/)

  ```C++
  class Solution {
  private:
      int list_len(ListNode* head){
          int len = 0;
          ListNode* cur = head;
          while (cur){
              len++;
              cur = cur->next;
          }
          return len;
      }
  
      vector<ListNode*> cut_list(ListNode* head, int a, int b){
          vector<ListNode*> cut(4, nullptr);
          cut[0] = head;
          ListNode* cur = head;
          for (int i = 0; i < a - 1; i++){
              cur = cur->next;
              cout << cur->val << endl;
          }
          ListNode* tail1 = cur;
          cout << "tail1 = " << tail1->val << endl;
          cut[1] = tail1;
          for (int i = 0; i < b - a + 1; i++){
              cur = cur->next;
          }
          ListNode* head2 = cur->next;
          cut[2] = head2;
          while (cur->next){
              cur = cur->next;
          }
          ListNode* tail2 = cur;
          //cout << head2->val << endl;
          // cout << tail1->val << endl;
          // cout << tail2->val << endl;
          tail1->next = nullptr;
          tail2->next = nullptr;
          return cut;
      }
  public:
      ListNode* mergeInBetween(ListNode* list1, int a, int b, ListNode* list2) {
          int n = list_len(list1);
          int m = list_len(list2);
          vector<ListNode*> tmp = cut_list(list1, a, b);
          tmp[1]->next = list2;
          auto cur2 = list2;
          while (cur2->next){
              cur2 = cur2->next;
          }
          cur2->next = tmp[2];
          return list1;
      }
  };
  
  
  //官解：思路简洁很多
  class Solution {
  public:
      ListNode* mergeInBetween(ListNode* list1, int a, int b, ListNode* list2) {
          ListNode* preA = list1;
          for (int i = 0; i < a - 1; i++){
              preA = preA->next;
          }
          ListNode* preB = preA;
          for (int i = 0; i < b - a + 2; i++){
              preB = preB->next;
          }
          preA->next = list2;
          while (list2->next){
              list2 = list2->next;
          }
          list2->next = preB;
          return list1;
      }
  };
  ```

  

- [2487. 从链表中移除节点 - 力扣（LeetCode）](https://leetcode.cn/problems/remove-nodes-from-linked-list/description/)

  ```C++
  class Solution {
  private:
      ListNode* reverse(ListNode* head){
          ListNode* pre = nullptr;
          ListNode* cur = head;
          while (cur){
              ListNode* tmp = cur->next;
              cur->next = pre;
              pre = cur;
              cur = tmp;
          }
          return pre;
      }
  public:
      ListNode* removeNodes(ListNode* head) {
          head = reverse(head);
          ListNode* cur = head;
          while (cur->next){
              if (cur->val > cur->next->val){
                  cur->next = cur->next->next;
              } else {
                  cur = cur->next;
              }
          }
          return reverse(head);
      }
  };
  ```

### 插入节点

- [2807. 在链表中插入最大公约数 - 力扣（LeetCode）](https://leetcode.cn/problems/insert-greatest-common-divisors-in-linked-list/description/)

  ```C++
  class Solution {
  private:
      int gcd(int a, int b){
          if (a % b == 0) return b;
          else return gcd(b, a % b);
      }
  public:
      ListNode* insertGreatestCommonDivisors(ListNode* head) {
          for (auto cur = head; cur->next; cur = cur->next->next){
              cur->next = new ListNode(gcd(cur->val, cur->next->val), cur->next);
          }
          return head;
      }
  };
  ```

  - 如何求最大公约数代码：[C++求最大公因数(gcd)的六重境界_c++ gcd-CSDN博客](https://blog.csdn.net/ceshyong/article/details/126211832)

    ```C++
    //迭代
    int gcd(int a,int b)
    {
    	while(b>0)
    	{
    		int r=a%b;
    		a=b;b=r;
    	}
    	return a;
    }
    //递归
    int gcd(int a,int b)
    {
        if(a%b==0) return b;
        else return gcd(b,a%b);
    }
    ```

- [147. 对链表进行插入排序 - 力扣（LeetCode）](https://leetcode.cn/problems/insertion-sort-list/description/)

  ```C++
  class Solution {
  public:
      ListNode* insertionSortList(ListNode* head) {
          if (!head) return head;
          ListNode* dummy = new ListNode(0);
          ListNode* cur = head;
          ListNode* pre = dummy;
          while (cur) {
              while (pre->next && pre->next->val < cur->val){
                  pre = pre->next;
              }
              auto nextNode = cur->next;
              cur->next = pre->next;
              pre->next = cur;
              pre = dummy;
              cur = nextNode;
          }
          return dummy->next;
      }
  };
  ```

  

- [LCR 029. 循环有序列表的插入 - 力扣（LeetCode）](https://leetcode.cn/problems/4ueAj6/description/)

  ```C++
  /*
  // Definition for a Node.
  class Node {
  public:
      int val;
      Node* next;
  
      Node() {}
  
      Node(int _val) {
          val = _val;
          next = NULL;
      }
  
      Node(int _val, Node* _next) {
          val = _val;
          next = _next;
      }
  };
  */
  
  class Solution {
  public:
      Node* insert(Node* head, int insertVal) {
          if (!head) {
              head = new Node(insertVal);
              head->next = head;
              return head;
          }
          auto cur =  head;
          while (cur->next != head){
              if (cur->next->val < cur->val){
                  if (cur->next->val >= insertVal) break;
                  else if (cur->val <= insertVal) break;
              }
   //情况讨论：1、在环形升序的情况下讨论，insertVal严格插入时需要前后都满足升序关系，而不严谨的情况下只需要符合前或后其一即可定cur
              if (cur->val <= insertVal && cur->next->val >= insertVal) break;
              cur = cur->next;
          }
          cur->next = new Node(insertVal, cur->next);
          return head;
      }
  };
  ```

  
