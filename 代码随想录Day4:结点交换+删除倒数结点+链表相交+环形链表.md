# [24.两两交换链表中的结点]([24. 两两交换链表中的节点 - 力扣（LeetCode）](https://leetcode.cn/problems/swap-nodes-in-pairs/))

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
  class Solution {
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



# [19.删除链表的倒数第N个节点]([19. 删除链表的倒数第 N 个结点 - 力扣（LeetCode）](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/))

## 题解思路





## 示例代码

# [面试题02.07.链表相交]([面试题 02.07. 链表相交 - 力扣（LeetCode）](https://leetcode.cn/problems/intersection-of-two-linked-lists-lcci/))

## 题解思路

## 示例代码

# [142.环形链表]([142. 环形链表 II - 力扣（LeetCode）](https://leetcode.cn/problems/linked-list-cycle-ii/))

## 题解思路

## 示例代码

