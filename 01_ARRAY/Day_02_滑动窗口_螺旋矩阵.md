## 滑动窗口

### 例题

[209.长度最小的子数组](https://leetcode.cn/problems/minimum-size-subarray-sum/description/)

#### 关键问题：最小 连续 子数组

*   **最小**：Result处置设置为INT32\_MAX

*   **连续**：滑动窗口 求解

#### 暴力解法

```C++
class Solution {
public:
   // 定义一个函数，用于找出给定目标值target在整数数组nums中的最小连续子数组长度
   int minSubArrayLen(int target, vector<int>& nums) {
       // 初始化结果为最大整数，用于后续比较获取最小值
       int result = INT32_MAX;
       // 遍历整数数组
       for(int i = 0; i<nums.size(); i++)
       {
           // 初始化子数组和和子数组长度
           int subSum = 0;
           int subSize = 0;
           // 从当前元素开始，向后遍历整数数组
           for (int j = i; j < nums.size(); j ++)
           {
               // 计算子数组和
               subSum += nums[j];
               // 如果子数组和大于等于目标值
               if (subSum >= target)
               {
                   // 计算子数组长度
                   subSize = j - i + 1;
                   // 更新结果为当前子数组长度和结果的较小值
                   result = subSize < result ? subSize:result;
                   // 找到一个满足条件的子数组后，跳出内层循环
                   break;
               }
           }
       }
       // 如果没有找到满足条件的子数组，返回0，否则返回结果
       return result != INT32_MAX ? result:0;
   }
};
```

*   时间复杂度：O(n^2)

*   空间复杂度：O(1)

#### 滑动窗口法

##### 本质

*   不断的调节子序列的起始位置和终止位置，从而得出我们要想的结果。

*   滑动窗口也可以理解为双指针法的一种

##### 与暴力解法

*   暴力解法：一个for循环滑动窗口的起始位置，一个for循环为滑动窗口的终止位置，用两个for循环完成一个不断搜索区间的过程

*   滑动窗口：只用一个for循环，那么这个循环的索引，一定是表示**滑动窗口的终止位置**&#x20;

##### 实现关键

*   窗口是什么：窗口就是满足其和 ≥ s 的长度最小的连续子数组

*   窗口起始位置移动：如果当前窗口的值大于等于s了，窗口就要向前移动了

*   窗口结束位置移动：窗口的结束位置就是循环遍历数组的指针

*   **精妙之处在于根据当前子序列和大小的情况，不断调节子序列的起始位置**

```C++
class Solution
{
public:
    // 定义一个函数，用于找出给定目标值target在整数数组nums中的最小连续子数组长度
    int minSubArrayLen(int target, vector<int> &nums)
    {
        // 初始化结果为最大整数，用于后续比较获取最小值
        int result = INT32_MAX;
        int subSum = 0;  // 子数组的和
        int subSize = 0; // 子数组的长度
        int i = 0;       // 子数组的起始位置
        for (int j = 0; j < nums.size(); j++)
        {
            subSum += nums[j];       // 计算当前子数组的和
            while (subSum >= target) // 当子数组的和大于等于目标值时
            {
                subSize = j - i + 1;                          // 计算子数组的长度
                result = subSize < result ? subSize : result; // 更新最小子数组长度
                // 滑动窗口的精髓之处，不断变更i（子序列的起始位置）
                subSum -= nums[i++]; // 移动子数组的起始位置，并更新子数组的和
            }
        }
        return result != INT32_MAX ? result : 0; // 如果没有找到满足条件的子数组，返回0
    }
};
```

*   时间复杂度：O(n)

    看每一个元素被操作的次数，每个元素在滑动窗后进来操作一次，出去操作一次，每个元素都是被操作两次，所以时间复杂度是 2 × n 也就是O(n)

*   空间复杂度：O(1)

### 相关题

#### [904.水果成篮](https://leetcode.cn/problems/fruit-into-baskets/)

#### [76.最小覆盖子串](https://leetcode.cn/problems/minimum-window-substring/)

## 螺旋矩阵

### 例题

[59.螺旋矩阵](https://leetcode.cn/problems/spiral-matrix-ii/)

#### 关键：坚持循环不变量原则

*   填充上行从左到右

*   填充右列从上到下

*   填充下行从右到左

*   填充左列从下到上

##### 左右边界限制解

```C++
class Solution {
public:
   // 生成一个n*n的螺旋矩阵
   vector<vector<int>> generateMatrix(int n) {
       // 初始化n*n的矩阵，所有元素值为0
       vector<vector<int>> result(n, vector<int>(n,0));
       // 定义四个边界，分别代表当前螺旋矩阵的上、下、左、右边界
       int Left = 0;
       int Right = n - 1;
       int Up = 0;
       int Down = n - 1;
       int i = 1;  // 用于填充矩阵的数字
       // 当矩阵填充完毕时停止循环
       while (i <= n*n)
       {
           // 从左到右填充上边界
           for (int j = Left; j <= Right ; j ++)
           {
               result[Up][j] = i++;
           }
           Up++;  // 上边界向下移动一位
           // 从上到下填充右边界
           for (int j = Up; j <= Down; j++)
           {
               result[j][Right] = i++;
           }
           Right--;  // 右边界向左移动一位
           // 从右到左填充下边界
           for (int j = Right; j >= Left; j--)
           {
               result[Down][j] = i++;
           }
           Down--;  // 下边界向上移动一位
           // 从下到上填充左边界
           for (int j = Down; j >= Up; j--)
           {
               result[j][Left] = i++;
           }
           Left++;  // 左边界向右移动一位
       }
       // 返回生成的螺旋矩阵
       return result;
   }
};
```

##### 标准解

```C++
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        // 初始化一个n x n的二维数组，所有元素初始化为0
        vector<vector<int>> res(n, vector<int>(n, 0));
        // 定义螺旋矩阵的起始位置 (startx, starty)，也可以理解为横纵坐标
        int startx = 0, starty = 0;
        // 循环的次数，n 为奇数时，中间会有一个单独的元素需要单独处理，所以除以2
        int loop = n / 2;
        // n为奇数时，中间元素的坐标，例如：n为3， 中间的位置就是(1，1)
        int mid = n / 2; 
        // 定义当前填充的数字，从 1 开始
        int count = 1; 
        // 定义每次圈数增加时的偏移量，每次循环右边界收缩一位
        int offset = 1;
        int i,j;

        // 开始螺旋填充，每填充完一圈，loop 减 1
        while (loop --) {
            // 当前圈的起始横坐标和纵坐标
            i = startx;
            j = starty;
    
            // 下面开始的四个for就是模拟转了一圈
            // 从左到右填充上边界(左闭右开)
            for (j = starty; j < n - offset; j++) {
                res[startx][j] = count++;
            }
            // 从上到下填充右边界(左闭右开)
            for (i = startx; i < n - offset; i++) {
                res[i][j] = count++;
            }
            // 从右到左填充下边界(左闭右开)
            for (; j > starty; j--) {
                res[i][j] = count++;
            }
            // 从下到上填充左边界(左闭右开)
            for (; i > startx; i--) {
                res[i][j] = count++;
            }
    
            // 更新下一圈的起始位置，例如：第一圈起始位置是(0, 0)，第二圈起始位置是(1, 1)
            startx++;
            starty++;
    
            // offset 控制每一圈里每一条边遍历的长度
            offset += 1;
        }
    
        // 如果 n 是奇数，则填充中心位置
        if (n % 2) {
            res[mid][mid] = count;
        }
        
        // 返回填充好的螺旋矩阵
        return res;
    }
};
```

### 类似题目

#### [54.螺旋矩阵](https://leetcode.cn/problems/spiral-matrix/)

#### [剑指Offer 29.顺时针打印矩阵](https://leetcode.cn/problems/shun-shi-zhen-da-yin-ju-zhen-lcof/)

