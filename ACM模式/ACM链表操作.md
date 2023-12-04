# [ACM反转链表](https://kamacoder.com/problempage.php?pid=1018)

```C++
#include <iostream>
#include <vector>
using namespace std;

struct ListNode{
    int val;
    ListNode* next;
    ListNode(int val) : val(val), next(nullptr) {}
};

ListNode* reverseList(ListNode* head){
    ListNode* cur = head;
    ListNode* tmp = nullptr;
    ListNode* pre = nullptr;
    while (cur){
        tmp = cur->next;
        cur->next = pre;
        pre = cur;
        cur = tmp;
    }
    return pre;
}

void printList(ListNode* head){
    ListNode* cur = head;
    while (cur){
        cout << cur->val << " ";
        cur = cur->next;
    }
    cout << endl;
}

int main(){
    int length, nodeVal;
    ListNode* dummyHead = new ListNode(0);
    while (cin >> length){
        if (length == 0){
            cout << "list is empty" << endl;
            continue;
        }
        ListNode* cur = dummyHead;
        while (length--){
            cin >> nodeVal;
            ListNode* node = new ListNode(nodeVal);
            cur->next = node;
            cur = cur->next;
        }
        printList(dummyHead->next);
        printList(reverseList(dummyHead->next));
    }
    return 0;
}
```

- ACM添加的部分：
  - 链表的定义
  - 反转链表函数
  - 自定义输入链表
  - 打印链表

