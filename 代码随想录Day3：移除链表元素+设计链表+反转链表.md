# [203.移除链表元素](https://leetcode.cn/problems/remove-linked-list-elements/)

## 题解思路

**1.链表操作的两种方式**<br>

- 直接使用原本的链表进行删除操作 （单独处理头结点）

  > 每个节点都是通过前继老操作下一个节点，所以前继判断到当前结点要删除了，前继的next直接指向后一个结点即可；而头结点则会因为没有前继需要单独操作。

- 设置一个虚拟头结点再进行删除操作（统一方式进行移除）

  > 虚拟头结点将头结点变成了常规结点，当完成对整个链表的操作过后将头结点重新还给原链表的头结点，删除虚拟头结点；
  >
  > 个人在写以下示例代码时使用了free和malloc操作时出现了地址报错，转而使用new来管理虚拟头结点，可以AC，在此询问GPT的相关内容放在此处 

  ```
  在C++中，使用`delete`和`free`都是用来释放动态分配的内存，但是它们之间有一些重要的区别。
  
  1. delete是C++的操作符，用于释放通过`new`运算符分配的单个对象或对象数组，而`free`是C语言中的函数，用于释放通过`malloc`、`calloc`或`realloc`函数分配的内存。
  
  2. delete会调用对象的析构函数，释放对象占用的内存，并且在释放内存之前会先调用析构函数进行资源的清理。而`free`只是简单地释放内存，不会调用对象的析构函数，也不会做任何资源的清理。
  
  3. 使用`delete`释放动态分配的对象数组时，需要使用`[]`，例如`delete[] objArray;`。而`free`不支持释放对象数组，只能逐个释放数组中的每个对象。
  
  4. 对于指针变量，`delete`将自动处理空指针的情况，并不会产生任何错误。而`free`不会对空指针进行处理，如果传递给`free`的是空指针，会产生未定义的行为。
  
  总结来说，使用`delete`能够提供更多的功能和保障，能够正确调用对象的析构函数，并释放对象占用的内存，适用于C++代码中的动态对象。而`free`更适合在C语言中使用，尤其是在需要手动管理内存的情况下。
  因此使用delete的问题少。
  ```

  **2.复杂度分析**<br>

  - 时间复杂度：O(n)
  - 空间复杂度：O(1)

  **3.注意易错点**<br>

  - 使用C++需要手动释放内存，上文也提及了使用delete来释放内存的内容

  - 使用虚拟头结点最后要将头结点重新还回去（如果直接return dummyHead->next是否可以？）

  - 删除节点的流程要背版

    ```C++
    if (cur->next->val == val){
    	ListNode* tmp = cur->next;
    	cur->next = cur->next->next;
        delete tmp;
    }
    ```

    

## 示例代码

```class Solution {
//This is a operating Original Linked List code
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        while (head != NULL && head->val == val){
            ListNode* tmp = head;
            head = head->next;
            delete tmp; 
        }
        ListNode* cur = head;//iterate from head
        while (cur != NULL && cur->next != NULL){
            if (cur->next->val == val ){
                ListNode* tmp = cur->next;
                cur->next = cur->next->next;
                delete tmp;
            }else {
                cur = cur->next;
            }
        }
        return head;
    }
};
```



```
//This is a using dummy_head code
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        typedef struct ListNode ListNode;
        ListNode* dummyHead = new ListNode(0);//开辟一块内存给虚拟头结点，其中什么都不存储
        dummyHead->next = head;
        ListNode* cur = dummyHead;
        while (cur->next != NULL){
            if (cur->next->val == val){
                //remove element
                ListNode* tmp = cur->next;
                cur->next = cur->next->next;
                delete(tmp);
            }else {
                cur = cur->next;
            }
        }
        head = dummyHead->next;
        delete(dummyHead);
        return head;
    }
};
```

# [707.设计链表](https://leetcode.cn/problems/design-linked-list/)

## 题解思路

**1.链表功能**

- 获取链表的第index个节点的数值
- 在链表的最前面插入一个节点
- 在链表的最后面插入一个节点
- 在链表的第index个节点前面插入一个节点
- 删除链表的第index个节点

**2.注意事项**

- 注意虚拟头结点使不计算在size当中的
- 当进行index查找的时候注意是从_dummyHead开始的，dummyHead的序号=0，头结点序号为1，所以有cur=dummyHead
- 犯错：在Add Index部分复用了Tail的代码，导致重复添加，已经备注在示例当中

**3.复杂度分析**

- 时间复杂度：涉及index的部分为O(index)，其余位O(1)
- 空间复杂度：O(1)

## 示例代码

```C++
class MyLinkedList {
public:
    struct LinkedNode{
        int val;
        LinkedNode* next;
        LinkedNode(int val):val(val), next(nullptr){}
    };

    MyLinkedList() {
        _dummyHead = new LinkedNode(0);
        _size = 0;
    }
    
    int get(int index) {
        if (index < 0 || index > (_size - 1)){return -1;}
        LinkedNode* cur = _dummyHead->next;
        while (index--){
            cur = cur->next;
        }
        return cur->val;
    }
    
    void addAtHead(int val) {
        LinkedNode* newNode = new LinkedNode(val);
        newNode->next = _dummyHead->next;
        _dummyHead->next = newNode;
        _size++;
    }
    
    void addAtTail(int val) {
        LinkedNode* newNode = new LinkedNode(val);
        LinkedNode* cur= _dummyHead;
        while (cur->next != nullptr){
            cur = cur->next;
        }
        cur->next = newNode;
        _size++;
    }
    
    void addAtIndex(int index, int val) {
        if (index < 0 ) index = 0;
        if (index > _size) return;
        //Mistake !! Here I add a If (index == _size) addAtTail which cause it add twice at Tail
        LinkedNode* cur = _dummyHead;
        LinkedNode* newNode = new LinkedNode(val);
        while (index--){
            cur = cur->next;
        }
        newNode->next = cur->next;
        cur->next = newNode;
        _size++;
    }
    
    void deleteAtIndex(int index) {
        if (index >= _size || index < 0) return;
        LinkedNode* cur = _dummyHead;
        while (index--){
            cur = cur->next;
        }
        LinkedNode* tmp = cur->next;
        cur->next = cur->next->next;
        delete tmp;

        tmp = nullptr;
        _size--;
    }
    void printLinkedList(){
        LinkedNode* cur = _dummyHead;
        while (cur->next != nullptr){
            cout << cur->next->val << " ";
            cur = cur->next;
        }
        cout << endl;
    }
private:
    int _size;
    LinkedNode* _dummyHead;
};
```

# [206.反转链表](https://leetcode.cn/problems/reverse-linked-list/)

## 题解思路

**1.修改next指向**

如果重新定义新的链表，对空间内存非常浪费，通过改变next的指向就能够实现链表的反转，而不用重新定义一个链表。<br>

- 双指针法：使用一个指针指向前项结点，每次反转指向前项结点的时候更新当前结点和前项的位置
- 递归法：
  - 终止条件：cur为空
  - 递归参数：前项和cur
  - 单层循环：更新前项和cur的指向
    
**2.复杂度分析**

- 时间复杂度（双指针）：O(n)
- 空间复杂度（双指针）：O(1)
- 时间复杂度（递归）：O(n)
- 空间复杂度（递归）：O(n)

## 示例代码

```C++
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* temp;
        ListNode* pre = nullptr;
        ListNode* cur = head;
        while (cur){
            temp = cur->next;
            cur->next = pre;
            pre = cur;
            cur = temp;
        }
        return pre;
    }
};
```

```C++
class Solution {
public:
    ListNode* reverse(ListNode* pre, ListNode* cur){
        if (cur == nullptr) return pre;
        ListNode* temp = cur->next;
        cur->next = pre;
        return reverse(cur, temp);
    }
    ListNode* reverseList(ListNode* head) {
        return reverse(nullptr, head);
    }
};
```

