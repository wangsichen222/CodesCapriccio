## 两两交换

### 例题

[24. 两两交换链表中的节点 - 力扣（LeetCode）](https://leetcode.cn/problems/swap-nodes-in-pairs/description/)

##### 虚拟头节点

```C++
class Solution {
public:
   // 定义一个函数，用于交换链表中的相邻节点
   ListNode* swapPairs(ListNode* head) {
       // 创建一个虚拟头节点，方便后续操作
       ListNode * dummyHead = new ListNode(0);
       // 将虚拟头节点的下一个节点设置为待交换的头节点
       dummyHead->next = head;
       // 初始化一个指针，指向虚拟头节点
       ListNode * cur = dummyHead;
       // 当当前节点的下一个节点和下一个节点的下一个节点都不为空时，进行交换
       while(cur->next != nullptr && cur->next->next != nullptr)
       {
           // 创建两个临时节点，分别指向当前节点的下一个节点和下一个节点的下一个节点
           ListNode * tmp1 = cur->next;
           ListNode * tmp2 = cur->next->next->next;
          
           // 将当前节点的下一个节点设置为下一个节点
           cur->next = tmp1->next;
           // 将下一个节点设置为当前节点的下一个节点
           cur->next->next = tmp1;
           // 将下一个节点的下一个节点设置为临时节点2
           cur->next->next->next = tmp2;

           // 将当前节点设置为下一个节点的下一个节点
           cur = cur->next->next;
       }
       // 将虚拟头节点的下一个节点赋值给结果，用于返回交换后的链表
       ListNode * result = dummyHead->next;
       // 删除虚拟头节点
       delete dummyHead;
      
       // 返回交换后的链表
       return result;
   }
};
```

*   时间复杂度：O(n)

*   空间复杂度：O(1)

## 删除链表的倒数第N个节点

### 例题

[19. 删除链表的倒数第 N 个结点 - 力扣（LeetCode）](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/description/)

##### 双指针法

*   如果要删除倒数第n个节点，让fast移动n步，然后让fast和slow同时移动，直到fast指向链表末尾

```C++
class Solution
{
public:
   // 定义一个函数，用于删除链表倒数第n个节点
   ListNode *removeNthFromEnd(ListNode *head, int n)
   {
       // 创建一个虚拟头节点，方便后续操作
       ListNode *dummyHead = new ListNode();
       // 将虚拟头节点的下一个节点指向头节点
       dummyHead->next = head;
       // 定义两个指针，fast指针先走n步
       ListNode *fast = dummyHead;
       ListNode *slow = dummyHead;
       while (n--)
       {
           fast = fast->next;
       }
       // 然后fast和slow指针同时走，当fast指针走到链表尾部时，slow指针停在倒数第n个节点的前一个节点
       while (fast->next != NULL)
       {
           slow = slow->next;
           fast = fast->next;
       }
       // 保存倒数第n个节点的指针
       ListNode *tmp = slow->next;
       // 删除倒数第n个节点
       slow->next = tmp->next;
       delete tmp;

       // 返回新的头节点
       return dummyHead->next;
   }
};
```

*   时间复杂度: O(n)

*   空间复杂度: O(1)

## 链表相交

### 例题

[面试题 02.07. 链表相交 - 力扣（LeetCode）](https://leetcode.cn/problems/intersection-of-two-linked-lists-lcci/description/)

##### 双指针法

```C++
class Solution
{
public:
   // 定义一个方法，用于获取两个链表的交点
   ListNode *getIntersectionNode(ListNode *headA, ListNode *headB)
   {
       ListNode *curA = headA;  // 定义一个指针curA，指向链表A的头结点
       ListNode *curB = headB;  // 定义一个指针curB，指向链表B的头结点

       int sizeA = 1;  // 定义一个变量sizeA，用于记录链表A的长度
       int sizeB = 1;  // 定义一个变量sizeB，用于记录链表B的长度

       // 计算链表A的长度
       while (curA->next != NULL)
       {
           curA = curA->next;
           sizeA++;
       }
       // 计算链表B的长度
       while (curB->next != NULL)
       {
           curB = curB->next;
           sizeB++;
       }

       curA = headA;  // 将curA重新指向链表A的头结点
       curB = headB;  // 将curB重新指向链表B的头结点

       // 将链表A和链表B移动到相同的起始点
       for (int AB = sizeA - sizeB; AB < 0; AB++)
       {
           curB = curB->next;
       }
       for (int AB = sizeA - sizeB; AB > 0; AB--)
       {
           curA = curA->next;
       }

       // 同步移动curA和curB，直到找到相交点或者都到达链表末尾
       while (curA != NULL)
       {
           if (curA == curB)
           {
               return curA;  // 返回交点
           }
           curA = curA->next;
           curB = curB->next;
       }
       return NULL;  // 如果两个链表没有交点，返回NULL
   }
};
```

## 环形链表

### 例题

[142. 环形链表 II - 力扣（LeetCode）](https://leetcode.cn/problems/linked-list-cycle-ii/description/)

#### 考察点

*   判断链表是否环

*   如果有环，如何找到这个环的入口

##### 双指针法

```C++
class Solution {
public:
   // 定义一个方法，用于检测链表中是否有环
   ListNode *detectCycle(ListNode *head) {
       ListNode* fast = head; // 定义一个快指针，初始位置与头结点相同
       ListNode* slow = head; // 定义一个慢指针，初始位置与头结点相同
       while(fast != NULL && fast->next != NULL) { // 当快指针和快指针的下一个节点都不为空时，继续循环
           slow = slow->next; // 慢指针每次前进一步
           fast = fast->next->next; // 快指针每次前进两步
           // 快慢指针相遇，此时从head 和 相遇点，同时查找直至相遇
           if (slow == fast) {
               ListNode* index1 = fast;
               ListNode* index2 = head;
               while (index1 != index2) { // 当两个指针不相等时，继续循环
                   index1 = index1->next; // 快指针每次前进一步
                   index2 = index2->next; // 慢指针每次前进一步
               }
               return index2; // 返回环的入口
           }
       }
       return NULL; // 如果没有环，返回NULL
   }
};
```

*   数学推导：**从头结点出发一个指针，从相遇节点也出发一个指针，这两个指针每次只走一个节点， 那么当这两个指针相遇的时候就是 环形入口的节点**

*   时间复杂度: O(n)，快慢指针相遇前，指针走的次数小于链表长度，快慢指针相遇后，两个index指针走的次数也小于链表长度，总体为走的次数小于 2n

*   空间复杂度: O(1)

