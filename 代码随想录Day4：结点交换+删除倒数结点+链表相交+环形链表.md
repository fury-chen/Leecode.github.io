# [24.两两交换链表中的结点](https://leetcode.cn/problems/swap-nodes-in-pairs/)

## 题解思路

**1.虚拟头结点**

整体思路是：

- cur = dummyHead;
- 使用cur操作cur->next和cur->next->next
- 每次cur后移两位

如果不使用虚拟头结点就要单独处理头结点，多写一个判断。

交换写法：

```C++
ListNode* tmp1 = cur->next;
ListNode* tmp2 = cur->nxet->next->next;
//以两个节点为单位处理，单位前后的连接要用临时变量保存
cur->next = cur->next->next;
cur->next->next = tmp1;
cur-next->next-next = tmp2;
```



**2.复杂度分析**

- 时间复杂度：O(n)

- 空间复杂度：O(1)

  

## 示例代码

```C++
lass Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        ListNode* dummyHead = new ListNode(0);
        dummyHead->next = head;
        ListNode* cur = dummyHead;
        while (cur->next != nullptr && cur->next->next != nullptr){
            ListNode* tmp1 = cur->next;
            ListNode* tmp2 = cur->next->next->next;

            cur->next = cur->next->next;
            cur->next->next = tmp1;
            cur->next->next->next = tmp2;

            cur = cur->next->next;
        }
        return dummyHead->next;
    }
};
```



# [19.删除链表的倒数第N个节点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)

## 题解思路

**1.虚拟头结点**

使用双指针：当fast指向target时，slow指向fast->next就能完成结点删除。

步骤：

- 使用虚拟头结点：方便删除实际头结点的逻辑
- fast先走n+1步，slow刚好指向第N个结点的上一个结点
- fast和slow同时移动，直到fast指向末尾
- 删除slow指向的下一个节点

**2.复杂度分析**

- 时间复杂度：O(n)
- 空间复杂度：O(1)

## 示例代码

```C++
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummyHead = new ListNode(0);
        dummyHead->next = head;
        ListNode* fastIndex = dummyHead;
        ListNode* slowIndex = dummyHead;
        while (n-- && fastIndex != nullptr){
            fastIndex = fastIndex->next;
        }
        fastIndex = fastIndex->next;
        while (fastIndex != nullptr){
            fastIndex = fastIndex->next;
            slowIndex = slowIndex->next;
        }
        slowIndex->next = slowIndex->next->next;
        //ListNode * tmp = slowIndex->next;
        //slow->next = tmp->next;
        //delet nth;
        return dummyHead->next;
    }
};
```



# [面试题02.07.链表相交](https://leetcode.cn/problems/intersection-of-two-linked-lists-lcci/)

## 题解思路

**1.指针相等**

- 本题特征是在交点之后所有节点都是重合的，所以先将末端对齐
- 使用两个指针在链表上进行查找，查到相等则找到交点
- 循环结束没找到则退出返回空指针

犯错：在计算长度后没有将curA重新赋值headA，导致在后续的while(gap--)中出现了访问空指针的情况

> runtime error: member access within null pointer of type 'ListNode' (solution.cpp)

**2.复杂度分析**

- 时间复杂度：O(n+m)
- 空间复杂度：O(1)

## 示例代码

```C++
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode* curA = headA;
        ListNode* curB = headB;
        int lenA = 0, lenB = 0;

        while (curA != NULL){
            lenA++;
            curA = curA->next;
        }
        while (curB != NULL){
            lenB++;
            curB = curB->next;
        }
        curA = headA;
        curB = headB;
        
        if (lenA < lenB){
            swap(lenA, lenB);
            swap(curA, curB);
        }
        int gap = lenA - lenB;
        while (gap--){
            curA = curA->next;
        }

        while (curA != NULL){
            if (curA == curB){
                return curA;
            }
            curA = curA->next;
            curB = curB->next;
        }
        return NULL;
    }
};
```



# [142.环形链表](https://leetcode.cn/problems/linked-list-cycle-ii/)

## 题解思路

**1.是否有环**：

- 使用快慢指针，fast每次移动两个节点，slow每次移动一个节点
- 如果fast和slow在途中相遇，则说明这个链表有环
- Why？——fast在环内总归会追上slow的，并且二者的速度差一直为1，不会被跳过去

**2.入口判断**

- 假设头结点到环的入口节点数为x，fast和slow的相遇节点为y，则可以得到以下关系

$$
(x + y) * 2 = x + y + n(y + z)\\
x + y = n(y + z)\\
x = (n - 1)(y + z) + z\\
$$

- 根据公式：两个节点分别从头结点和相遇节点出发，每次只走一步，两个指针会在入口处相遇

**2.复杂度分析**

- 时间复杂度：O(n)
- 空间复杂度：O(1)

## 示例代码

```C++
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode* fastIndex = head;
        ListNode* slowIndex = head;
        while (fastIndex != NULL && fastIndex->next != NULL){
            slowIndex = slowIndex->next;
            fastIndex = fastIndex->next->next;
            if (slowIndex == fastIndex){
                ListNode* index1 = fastIndex;
                ListNode* index2 = head;
                while (index1 != index2){
                    index1 = index1->next;
                    index2 = index2->next;
                }
                return index2;
            }
        }
        return NULL;
    }
};
```

