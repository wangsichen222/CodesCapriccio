# 数组理论基础

**数组是存放在连续内存空间上的相同类型数据的集。**

*   **数组下标都是从0开始的**

*   **数组内存空间的地址是连续的（在删除或者增添元素的时候，需移动其他元素的地址）**

*   **vector 和 array的区别：** vector的底层实现是array，严格来讲vector是容器，不是数组

*   **数组的元素是不能删的，只能覆盖**

*   **二维数组在内存的空间地址是连续的么？在C++中二维数组是连续分布的**

# 二分查找

## 例题：

[704.二分查找](https://leetcode.cn/problems/binary-search/)


### **关键问题：区间定义**

*   左闭右闭：**\[left, right]**

*   左闭右开：**\[left, right)**

### 左闭右闭：**\[left, right] **

*   **while (left <\= right) 要使用 <\=** ，因为left \=\= right是有意义的，所以使用 <\=

*   **if (nums\[middle] > target) right 更新为 middle - 1**，因为当前这个nums\[middle]一定不是target，那么接下来要查找的左区间结束下标位置就是 middle - 1

```C++
class Solution
{
public:
   // 定义一个二分查找函数，输入为一个整数数组和一个目标值，返回目标值在数组中的索引，如果不存在则返回-1
   int search(vector<int> &nums, int target)
   {
       // 初始化左右指针
       int left = 0;
       int right = nums.size() - 1;

       // 当左指针小于等于右指针时，进行循环
       while (left <= right)
       {
           // 计算中间位置的索引
           int middle = left + ((right - left) / 2);

           // 如果中间位置的值大于目标值，将右指针移动到中间位置减一的位置
           if (nums[middle] > target)
           {
               right = middle - 1;
           }
           // 如果中间位置的值小于目标值，将左指针移动到中间位置加一的位置
           else if (nums[middle] < target)
           {
               left = middle + 1;
           }
           // 如果中间位置的值等于目标值，返回中间位置的索引
           else
           {
               return middle;
           }
       }
       // 如果遍历结束后未找到目标值，返回-1
       return -1;
   }
};
```

*   时间复杂度：O(log n)

*   空间复杂度：O(1)

### 左闭右开：**\[left, right)**

*   **while (left < right)，这里使用 <** ,因为left \=\= right在区间\[left, right)是没有意义的

*   **if (nums\[middle] > target) right 更新为 middle**，因为当前nums\[middle]不等于target，去左区间继续寻找，而寻找区间是左闭右开区间，所以right更新为middle，即：下一个查询区间不会去比较nums\[middle]

```CPP
// 版本二
class Solution
{
public:
    int search(vector<int> &nums, int target)
    {
        int left = 0;
        int right = nums.size(); // 定义target在左闭右开的区间里，即：[left, right)
        while (left < right)
        {                                              // 因为left == right的时候，在[left, right)是无效的空间，所以使用 <
            int middle = left + ((right - left) >> 1); // 左移一位 == /2
            if (nums[middle] > target)
            {
                right = middle; // target 在左区间，在[left, middle)中
            }
            else if (nums[middle] < target)
            {
                left = middle + 1; // target 在右区间，在[middle + 1, right)中
            }
            else
            {                  // nums[middle] == target
                return middle; // 数组中找到目标值，直接返回下标
            }
        }
        // 未找到目标值
        return -1;
    }
};

```

*   时间复杂度：O(log n)

*   空间复杂度：O(1)

## 相关题：

#### [35.搜索插入位置](https://programmercarl.com/0035.%E6%90%9C%E7%B4%A2%E6%8F%92%E5%85%A5%E4%BD%8D%E7%BD%AE.html)

```C++
class Solution
{
public:
   // 定义一个函数，用于在排序数组中查找元素，如果找到就返回该元素的索引，否则返回应该插入该元素的位置
   int searchInsert(vector<int> &nums, int target)
   {
       int left = -1;  // 初始化左边界为-1
       int right = nums.size();  // 初始化右边界为数组长度
       while (left + 1 != right)
       { // 当左边界小于右边界时，继续循环
           int mid = (left + right) / 2;  // 计算中间位置
           // 如果中间位置的元素大于等于目标值，则将右边界移动到中间位置
           if (nums[mid] >= target)
               right = mid;
           // 如果中间位置的元素小于目标值，则将左边界移动到中间位置
           else
               left = mid;
       }
       // 循环结束后，返回右边界，即应该插入的位置
       return right;
   }
};
```

#### [34.在排序数组中查找元素的第一个和最后一个位置](https://programmercarl.com/0034.%E5%9C%A8%E6%8E%92%E5%BA%8F%E6%95%B0%E7%BB%84%E4%B8%AD%E6%9F%A5%E6%89%BE%E5%85%83%E7%B4%A0%E7%9A%84%E7%AC%AC%E4%B8%80%E4%B8%AA%E5%92%8C%E6%9C%80%E5%90%8E%E4%B8%80%E4%B8%AA%E4%BD%8D%E7%BD%AE.html)

#### [69.x 的平方根](https://leetcode.cn/problems/sqrtx/)

```C++
class Solution
{
public:
    int mySqrt(int x)
    {
        return pow(x, 0.5);
    }
};
```

#### [367.有效的完全平方数](https://leetcode.cn/problems/valid-perfect-square/)

# 移除元素

## 例题：

[27.移除元素](https://leetcode.cn/problems/remove-element/)

### **关键问题：数组内存地址连续**

数组的元素在内存地址中是连续的，**不能**单独**删除**数组中的某个元素，**只能覆盖**。

### 暴力解法

双循环，递归寻找

```C++
class Solution
{
public:
   // 定义一个公共函数，用于移除数组中的特定元素
   int removeElement(vector<int> &nums, int val)
   {
       // 获取数组的大小
       int size = nums.size();
       // 遍历数组
       for (int i = 0; i < size; i++)
       {
           // 如果当前元素等于目标值
           if (nums[i] == val)
           {
               // 从当前元素的下一个元素开始，向左移动元素
               for (int j = i + 1; j < size; j++)
               {
                   nums[j - 1] = nums[j];
               }
               // 因为右侧元素都向左移动了一位，所以当前位置的元素已经被处理，i减1
               i--;
               // 数组大小减1，因为右侧元素都向左移动了一位
               size--;
           }
       }
       // 返回处理后的数组大小
       return size;
   }
};
```

*   时间复杂度：O(n^2)

*   空间复杂度：O(1)

### 双指针法

双指针法（快慢指针法）： 通过一个快指针和慢指针在**一个for循环下完成两个for循环的工作**

*   快指针：寻找新数组的元素 ，新数组就是不含有目标元素的数组

*   慢指针：指向更新 新数组下标的位置、

双指针法（快慢指针法）在数组和链表的操作中是**非常常见**的，很多考察数组、链表、字符串等操作的面试题，都使用双指针法。

```C++
class Solution
{
public:
   // 定义一个公共函数，用于移除数组中的特定元素
   // 参数: nums - 输入的整数数组
   //       val - 需要移除的特定元素
   // 返回值: 移除元素后的数组长度
   int removeElement(vector<int> &nums, int val)
   {
       int size = nums.size(); // 获取原始数组长度
       int slowIndex = 0; // 定义慢指针，用于记录新数组的下标
       // 双指针遍历数组
       for (int fastIndex = 0; fastIndex < size; fastIndex++)
       {
           // 如果当前元素不等于val，则将其复制到新数组的相应位置
           if (nums[fastIndex] != val)
           {
               nums[slowIndex] = nums[fastIndex];
               slowIndex++; // 慢指针向后移动
           }
       }
       return slowIndex; // 返回移除元素后的数组长度
   }
};
```

*   时间复杂度：O(n)

*   空间复杂度：O(1)

## 相关题：

### [26.删除排序数组中的重复项](https://leetcode.cn/problems/remove-duplicates-from-sorted-array/)

### [283.移动零](https://leetcode.cn/problems/move-zeroes/)

### [844.比较含退格的字符串](https://leetcode.cn/problems/backspace-string-compare/)

### [977.有序数组的平方](https://leetcode.cn/problems/squares-of-a-sorted-array/)
