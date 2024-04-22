## 链表基础知识

### 链表定义

*   链表是一种通过指针串联在一起的线性结构

*   每一个节点由两部分组成，一个是数据域一个是指针域（存放指向下一个节点的指针），最后一个节点的指针域指向null

*   链表的入口节点称为链表的头结点也就是head

![](02_LINKED_LIST_md_files/ce3b8bc0-004f-11ef-b744-43bd4c91b6cc.jpeg?v=1\&type=image)

### 链表类型

#### 单链表

*   单链表中的指针域只能指向节点的下一个节点

#### 双链表

*   每一个节点有两个指针域，一个指向下一个节点，一个指向上一个节点

*   既可以向前查询也可以向后查询

![](02_LINKED_LIST_md_files/c8601b20-0050-11ef-b744-43bd4c91b6cc.jpeg?v=1\&type=image)

#### 循环链表

*   链表首尾相连，可以用来解决约瑟夫环问题

![](02_LINKED_LIST_md_files/df35e690-0050-11ef-b744-43bd4c91b6cc.jpeg?v=1\&type=image)

### 存储方式

*   通过指针域的指针链接在内存中各个节点，在内存中不是连续分布的

*   链表中的节点散乱分布在内存中的某地址上，分配机制取决于操作系统的内存管理

### 程序定义

```C++
// 单链表
// 定义一个链表节点结构体
struct ListNode {
   int val;  // 节点值
   ListNode *next;  // 指向下一个节点的指针

   // 默认构造函数，初始化节点值为0，下一个节点为空
   ListNode() : val(0), next(nullptr) {}

   // 构造函数，接收一个整数作为节点值，初始化下一个节点为空
   ListNode(int x) : val(x), next(nullptr) {}

   // 构造函数，接收一个整数和一个节点指针，作为当前节点的值和下一个节点
   ListNode(int x, ListNode *next) : val(x), next(next) {}
};
```

通过定**义的构造函数**初始化节点：

```C++
ListNode* head = new ListNode(5);
```

使用默认构造函数初始化节点：

```C++
ListNode* head = new ListNode();
head->val = 5;
```

*   如果不定义构造函数使用默认构造函数的话，在初始化的时候就不能直接给变量赋值

### 性能对比

![](02_LINKED_LIST_md_files/4c6e25a0-0052-11ef-b744-43bd4c91b6cc.jpeg?v=1\&type=image)

*   数组长度就是固定的，如果想改动数组的长度，就需要重新定义一个新的数组

*   链表的长度可以是不固定的，并且可以动态增删， 适合数据量不固定，频繁增删，较少查询的场景

## 移除链表元素

### 例题

[203. 移除链表元素 - 力扣（LeetCode）](https://leetcode.cn/problems/remove-linked-list-elements/)

#### 内存清理

使用C++来做leetcode，如果移除一个节点之后，没有手动在内存中删除这个节点，leetcode依然也是可

以通过的，只不过，内存使用的空间大一些而已，但建议依然要养成**手动清理内存**的习惯

#### 节点移除

*   中间节点

    让**节点next指针直接指向下下一个节点**

*   头节点

    *   直接使用**原链表**来进行删除操作（head后移，清空原节点内存）

    *   设置一个**虚拟头结点**在进行删除操作

##### 分类移除链表元素

```C++
class Solution
{
public:
    ListNode *removeElements(ListNode *head, int val)
    {
        // 删除头节点
        // 当头节点的值等于给定值时，将头节点指向下一个节点，并删除当前头节点
        while (head != NULL && head->val == val)
        {
            ListNode *tmp = head; // 创建临时节点保存当前头节点
            head = head->next;    // 将头节点移动到下一个节点
            delete tmp;           // 删除临时节点，释放内存
        }

        // 删除非头节点
        ListNode *cur = head; // 创建一个当前节点指针，从头节点开始
        while (cur != NULL && cur->next != NULL)
        { // 当当前节点和下一个节点都不为空时
            if (cur->next->val == val)
            {                              // 如果下一个节点的值等于给定值
                ListNode *tmp = cur->next; // 创建临时节点保存下一个节点
                // cur->next = cur->next->next;  // 将当前节点的next指向下下一个节点
                cur->next = tmp->next; // 将当前节点的next指针跳过下一个节点
                delete tmp;            // 删除临时节点，释放内存
            }
            else
            {
                cur = cur->next; // 如果下一个节点的值不等于给定值，将当前节点移动到下一个节点
            }
        }
        return head; // 返回处理后的链表头节点
    }
};
```

*   时间复杂度: O(n)

*   空间复杂度: O(1)

##### 虚拟头节点移除链表元素

```C++
class Solution {
public:
   // 删除链表中所有值为val的节点
   ListNode* removeElements(ListNode* head, int val) {
       // 创建一个虚拟头节点，方便处理链表头部的特殊情况
       ListNode * dummyHead = new ListNode(0);
       // 将虚拟头节点的下一个节点指向链表的头节点
       dummyHead->next = head;
       // 初始化一个指向虚拟头节点的指针
       ListNode * cur = dummyHead;
       // 当当前指针不为空且其下一个节点不为空时，继续循环
       while(cur != NULL && cur->next !=NULL)
       {
           // 如果当前节点的下一个节点的值等于val，则删除该节点
           if (cur->next->val == val)
           {
               // 创建一个临时节点，指向待删除节点
               ListNode * tmp = cur->next;
               // 将当前节点的下一个节点指向待删除节点的下一个节点
               cur->next = tmp->next;
               // 删除待删除节点
               delete tmp;
           }
           else{
               // 如果当前节点的下一个节点的值不等于val，则将当前节点移动到下一个节点
               cur = cur->next;
           }
       }
       // 将虚拟头节点的下一个节点赋值给head，这是删除操作后的新的链表头节点
       head = dummyHead->next;
       // 删除虚拟头节点
       delete dummyHead;

       // 返回删除操作后的新的链表头节点
       return head;
   }
};
```

*   时间复杂度: O(n)

*   空间复杂度: O(1)

## 设计链表

### 例题

[707. 设计链表 - 力扣（LeetCode）](https://leetcode.cn/problems/design-linked-list/)

#### 设计目标

*   get(index)：获取链表中第 index 个节点的值。如果索引无效，则返回-1。

*   addAtHead(val)：在链表的第一个元素之前添加一个值为 val 的节点。插入后，新节点将成为链表的第一个节点。

*   addAtTail(val)：将值为 val 的节点追加到链表的最后一个元素。

*   addAtIndex(index,val)：在链表中的第 index 个节点之前添加值为 val  的节点。如果 index 等于链表的长度，则该节点将附加到链表的末尾。如果 index 大于链表长度，则不会插入节点。如果index小于0，则在头部插入节点。

*   deleteAtIndex(index)：如果索引 index 有效，则删除链表中的第 index 个节点。

##### 虚拟头节点法

```C++
class MyLinkedList
{
public:
   struct LinkedNode //定义链表节点结构，包含值和下一个节点的指针
   {
       int val;
       LinkedNode *next;
       LinkedNode(int val) : val(val), next(nullptr) {} //构造函数，初始化节点值和next指针
   };

   MyLinkedList() // 构造函数
   {
       _dummyHead = new LinkedNode(0); // 创建一个虚拟头节点，方便操作
       _size = 0; // 初始链表为空
   }

   int get(int index) // 获取链表指定位置的值
   {
       if (index >= _size || index < 0) // 如果索引无效，返回-1
       {
           return -1;
       }
       LinkedNode *cur = _dummyHead->next; // 从头节点的下一个节点开始遍历
       while (index--) // 遍历到指定位置
       {
           cur = cur->next;
       }
       return cur->val; // 返回指定位置的值
   }

   void addAtHead(int val) // 在链表头部添加节点
   {
       LinkedNode *tmp = new LinkedNode(val); // 创建新节点
       tmp->next = _dummyHead->next; // 新节点的下一个节点指向头节点的下一个节点
       _dummyHead->next = tmp; // 头节点的下一个节点变为新节点
       _size++; // 链表节点数量加1
   }

   void addAtTail(int val) // 在链表尾部添加节点
   {
       LinkedNode *tmp = new LinkedNode(val); // 创建新节点
       LinkedNode *cur = _dummyHead; // 从头节点开始遍历
       while (cur->next != nullptr) // 遍历到尾节点
       {
           cur = cur->next;
       }
       cur->next = tmp; // 尾节点的下一个节点变为新节点
       _size++; // 链表节点数量加1
   }

   void addAtIndex(int index, int val) // 在链表指定位置添加节点
   {
       if (index > _size || index < 0) // 如果索引无效，直接返回
       {
           return;
       }
       LinkedNode *tmp = new LinkedNode(val); // 创建新节点
       LinkedNode *cur = _dummyHead; // 从头节点开始遍历
       while (index--) // 遍历到指定位置
       {
           cur = cur->next;
       }
       tmp->next = cur->next; // 新节点的下一个节点变为指定位置的节点
       cur->next = tmp; // 指定位置的节点变为新节点
       _size++; // 链表节点数量加1
   }

   void deleteAtIndex(int index) // 删除链表指定位置的节点
   {
       if (index >= _size || index < 0) // 如果索引无效，直接返回
       {
           return;
       }
       LinkedNode *cur = _dummyHead; // 从头节点开始遍历
       while (index--) // 遍历到指定位置
       {
           cur = cur->next;
       }
       LinkedNode *tmp = cur->next; // 保存指定位置的节点
       cur->next = cur->next->next; // 指定位置的节点变为其下一个节点
       delete tmp; // 删除指定位置的节点

       // delete命令释放了tmp指针所指的内存，此时tmp是一个野指针，
       // 如果不设置为nullptr，可能出现使用错误的情况。
       tmp = nullptr;  
       _size--; // 链表节点数量减1
   }

   void printLinkedList() // 打印链表所有元素
   {
       LinkedNode *cur = _dummyHead; // 从头节点开始遍历
       while (cur->next != nullptr) // 遍历到尾节点
       {
           cout << cur->next->val << " "; // 打印节点值
           cur = cur->next;
       }
       cout << endl;
   }

private:
    int _size;
    LinkedNode *_dummyHead;
};
```

*   时间复杂度: 涉及 `index` 的相关操作为 O(index), 其余为 O(1)

*   空间复杂度: O(n)

## 反转链表

### 例题

[206. 反转链表 - 力扣（LeetCode）](https://leetcode.cn/problems/reverse-linked-list/description/)

#### 思路

*   再定义一个新的链表，实现链表元素的反转，其实这是对内存空间的浪费。

*   只需要改变链表的next指针的指向，直接将链表反转 ，而不用重新定义一个新的链表

##### 双指针法

```C++
class Solution
{
public:
   // 定义一个函数，用于反转链表
   ListNode *reverseList(ListNode *head)
   {
       ListNode *pre = NULL;  // 定义一个指向null的指针pre，用于存储反转后的链表的头结点
       ListNode *cur = head;  // 定义一个指向输入链表头结点的指针cur
       ListNode *tmp;  // 定义一个临时指针tmp，用于存储当前节点的下一节点
       while(cur)  // 当cur不为null时，循环执行
       {
           tmp = cur->next;  // 将cur的下一节点赋值给tmp
           cur->next = pre;  // 将cur的下一节点指向pre，即反转cur的指针

           pre = cur;  // 将pre指向cur，准备反转下一节点
           cur = tmp;  // 将cur指向tmp，准备处理下一节点
       }
       return pre;  // 返回反转后的链表头结点
   }
};
```

*   向后指向的指针旋转180°

*   时间复杂度: O(n)

*   空间复杂度: O(1)

##### 递归法

```cpp
class Solution
{
public:
    ListNode *reverse(ListNode *pre, ListNode *cur)
    {
        if (cur == NULL)
            return pre;
        ListNode *temp = cur->next;
        cur->next = pre;
        // 可以和双指针法的代码进行对比，如下递归的写法，其实就是做了这两步
        // pre = cur;
        // cur = temp;
        return reverse(cur, temp);
    }

    ListNode *reverseList(ListNode *head)
    {
        // 和双指针法初始化是一样的逻辑
        // ListNode* cur = head;
        // ListNode* pre = NULL;
        return reverse(NULL, head);
    }
};
```

*   时间复杂度: O(n), 要递归处理链表的每个节点

*   空间复杂度: O(n), 递归调用了 n 层栈空间
