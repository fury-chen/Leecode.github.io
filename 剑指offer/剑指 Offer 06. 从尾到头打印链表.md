# [剑指 Offer 06. 从尾到头打印链表](https://www.acwing.com/problem/content/18/)

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
    vector<int> printListReversingly(ListNode* head) {
        vector<int> result;
        stack<int> st;
        //if (head == NULL) return -1;
        ListNode* cur = head;
        while (cur != NULL){
            st.push(cur->val);
            cur = cur->next;
        }
        while (!st.empty()){
            int tmp = st.top();
            result.push_back(tmp);
            st.pop();
        }
        return result;
    }
};
```

- 入栈之后的数据变成逆序，逆序的一个处理模板

