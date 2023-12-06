# [33. 链表中倒数第k个节点 - AcWing题库](https://www.acwing.com/problem/content/description/32/)



## 示例代码

- 使用双指针的思路，fast多移动一格；
- 加入了len 的逻辑判断k与len的关系

```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* findKthToTail(ListNode* pListHead, int k) {
        ListNode* dummyHead = new ListNode(-1);
        dummyHead->next = pListHead;
        
        auto cur = dummyHead;
        int len = -1;
        while (cur != NULL){
            len++;
            cur = cur->next;
        }
        //cout << len << endl;
        if (len < k) return NULL;
        
        ListNode* fastIndex = dummyHead->next;
        ListNode* slowIndex = dummyHead;
        while (k--){
            fastIndex = fastIndex->next;
        }
        while (fastIndex != NULL){
            fastIndex = fastIndex->next;
            slowIndex = slowIndex->next;
        }
        
        return slowIndex->next;
    }
};
```

