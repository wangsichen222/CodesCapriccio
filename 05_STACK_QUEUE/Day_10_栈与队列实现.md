## 栈与队列基础理论

![](05_STACK_QUEUE_md_files/c5488950-068f-11ef-a229-7bf23699d131.jpeg?v=1\&type=image)

### 栈

1.  C++中stack 是容器么？

    栈和队列是STL（C++标准库）里面的两个数据结构

2.  我们使用的stack是属于哪个版本的STL？

    三个最为普遍的STL版本：

    1.  HP STL 其他版本的C++ STL，一般是以HP STL为蓝本实现出来的，HP STL是C++ STL的第一个实现版本，而且开放源代码；

    2.  P.J.Plauger STL 由P.J.Plauger参照HP STL实现出来的，被Visual C++编译器所采用，不是开源的；

    3.  **SGI STL** 由Silicon Graphics Computer Systems公司参照HP STL实现，被Linux的C++编译器GCC所采用，SGI STL是开源软件，源码可读性甚高

3.  我们使用的STL中stack是如何实现的？

    ![](05_STACK_QUEUE_md_files/62bc8000-0691-11ef-a229-7bf23699d131.jpeg?v=1\&type=image)

    *   栈的底层实现可以是vector，deque，list 都是可以的， 主要就是数组和链表的底层实现

    *   **常用的SGI STL，如果没有指定底层实现的话，默认是以deque为缺省情况下栈的底层结构**

    *   deque是一个双向队列，只要封住一段，只开通另一端就可以实现栈的逻辑了

    *   **SGI STL中 队列底层实现缺省情况下一样使用deque实现的**

    指定vector为栈的底层实现，初始化语句如下：

    ```C++
    std::stack<int, std::vector<int> > third;  // 使用vector为底层容器的栈
    ```

4.  stack 提供迭代器来遍历stack空间么？

    ![](05_STACK_QUEUE_md_files/6e230c70-0691-11ef-a229-7bf23699d131.jpeg?v=1\&type=image)

    *   栈提供push 和 pop 等等接口，所有元素必须符合先进后出规则

    *   所以栈不提供走访功能，也不提供迭代器(iterator)，不像是set 或者map 提供迭代器iterator来遍历所有元素

    *   **栈是以底层容器完成其所有的工作，对外提供统一的接口，底层容器是可插拔的（也就是说我们可以控制使用哪种容器来实现栈的功能）**

    *   &#x20;所以STL中栈往往不被归类为容器，而被归类为container adapter（容器适配器）

### 队列

*   队列中先进先出的数据结构，同样不允许有遍历行为，不提供迭代器

*   **SGI STL中队列一样是以deque为缺省情况下的底部结构**

也可以指定list 为起底层实现，初始化queue的语句如下：

```C++
std::queue<int, std::list<int>> third; // 定义以list为底层容器的队列
```

所以STL 队列也不被归类为容器，而被归类为container adapter（ 容器适配器）



## 用栈模拟队列

### 例题

[232. 用栈实现队列 - 力扣（LeetCode）](https://leetcode.cn/problems/implement-queue-using-stacks/description/)

### 关键

需要两个栈**一个输入栈，一个输出栈**

*   在push数据的时候，只要数据放进输入栈就好

*   **但在pop的时候，操作就复杂一些，输出栈如果为空，就把进栈数据全部导入进来（注意是全部导入）**，再从出栈弹出数据，如果输出栈不为空，则直接从出栈弹出数据就可以了

*   何判断队列为空呢？**如果进栈和出栈都为空的话，说明模拟的队列为空了**

```C++
class MyQueue
{
public:
   stack<int> stIn;  // 创建一个栈stIn用于存储入栈的元素
   stack<int> stOut; // 创建一个栈stOut用于存储出栈的元素

   MyQueue()
   {
   }

   void push(int x)  // 定义push函数，用于将元素x入栈
   {
       stIn.push(x); // 将元素x压入stIn栈中
   }

   int pop()  // 定义pop函数，用于将元素从队列中出栈
   {
       if (stOut.empty()) // 如果stOut栈为空
       {
           while (!stIn.empty()) // 如果stIn栈不为空
           {
               stOut.push(stIn.top()); // 将stIn栈顶元素压入stOut栈
               stIn.pop(); // 将stIn栈顶元素弹出
           }
       }
       int result = stOut.top(); // 将stOut栈顶元素赋值给result
       stOut.pop(); // 将stOut栈顶元素弹出
       return result; // 返回result
   }

   int peek()  // 定义peek函数，用于查看队列头部元素
   {
       int result = this->pop(); // 调用pop函数，将队列头部元素赋值给result
       stOut.push(result); // 将result压入stOut栈
       return result; // 返回result
   }

   bool empty()  // 定义empty函数，用于判断队列是否为空
   {
       return stIn.empty() && stOut.empty(); // 如果stIn栈和stOut栈都为空，返回true，否则返回false
   }
};
```

*   时间复杂度: push和empty为O(1), pop和peek为O(n)

*   空间复杂度: O(n)

## 用队列模拟栈

### 例题

[225. 用队列实现栈 - 力扣（LeetCode）](https://leetcode.cn/problems/implement-stack-using-queues/description/)

### 关键

**一个队列在模拟栈弹出元素的时候只要将队列头部的元素（除了最后一个元素外） 重新添加到队列尾部，此时再去弹出元素就是栈的顺序了**

##### 单队列实现

```C++
class MyStack {
public:
   queue<int> que1;  // 使用队列que1作为堆栈的存储结构

   MyStack() {  // 构造函数，初始化堆栈

   }
   
   void push(int x) {  // 压栈操作
       que1.push(x);  // 将元素x压入队列
   }
   
   int pop() {  // 弹栈操作
       int size = que1.size();  // 获取队列的大小
       size--;  // 减小一个元素
       while (size-- )  // 将队列中的元素依次向队尾移动一位
       {
           que1.push(que1.front());  // 将队首元素压入队尾
           que1.pop();  // 弹出队首元素
           // que1.push(que1.pop()); // 错误：push(const value)
       }
       int result = que1.front();  // 获取队首元素，即原队尾元素
       que1.pop();  // 弹出队首元素
       return result;  // 返回弹出的元素
   }
   
   int top() {  // 获取栈顶元素
       return que1.back();  // 返回队尾元素，即原栈顶元素
   }
   
   bool empty() {  // 判断栈是否为空
       return que1.empty();  // 返回队列是否为空
   }
};
```

*   时间复杂度: pop为O(n)，其他为O(1)

*   空间复杂度: O(n)

