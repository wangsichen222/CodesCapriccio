## 哈希表基础理论

### 哈希表

*   哈希表是根据**关键码的值(参考数组下标)**而直接进行访问的数据结构

*   一般哈希表都是用来快速判断一个元素是否出现集合里

例如要查询一个名字是否在这所学校里

要枚举的话时间复杂度是O(n)，但如果使用哈希表的话， 只需要O(1)就可以做到

我们只需要初始化把这所学校里学生的名字都存在哈希表里，在查询的时候通过索引直接就可以知道这位同学在不在这所学校里了

将学生姓名映射到哈希表上就涉及到了**hash function ，也就是哈希函数**

### 哈希函数

哈希函数，把学生的姓名直接映射为哈希表上的索引，然后就可以通过查询索引下标快速知道这位同学是否在这所学校里了

哈希函数如下图所示，通过hashCode把名字转化为数值，一般hashcode是通过特定编码方式，可以将其他数据格式转化为不同的数值，这样就把学生名字映射为哈希表上的索引数字了

![](03_HASH_TABLE_md_files/2d4916e0-00b5-11ef-82f4-85dfb8513ca6.jpeg?v=1\&type=image)

此时为了保证映射出来的索引数值都落在哈希表上，我们会在再次对数值做一个取模的操作，这样我们就保证了学生姓名一定可以映射到哈希表上了

### 哈希碰撞

如图所示，小李和小王都映射到了索引下标 1 的位置，**这一现象叫做哈希碰撞**&#x20;

![](03_HASH_TABLE_md_files/73f659e0-00b5-11ef-82f4-85dfb8513ca6.jpeg?v=1\&type=image)

#### 解法

##### 拉链法

*   其实拉链法就是要选择适当的哈希表的大小，这样既不会因为数组空值而浪费大量内存，也不会因为链表太长而在查找上浪费太多时间

![](03_HASH_TABLE_md_files/9bdf7bd0-00b5-11ef-82f4-85dfb8513ca6.jpeg?v=1\&type=image)

##### 线性探测法

*   一定要保证tableSize大于dataSize

    例如冲突的位置，放了小李，那么就向下找一个空位放置小王的信息。所以要求tableSize一定要大于dataSize ，要不然哈希表上就没有空置的位置来存放 冲突的数据了

![](03_HASH_TABLE_md_files/b03db790-00b5-11ef-82f4-85dfb8513ca6.jpeg?v=1\&type=image)

### 三种常见的哈希结构

#### 数组

#### set（集合）

##### 底层实现

*   std::unordered\_set底层实现为哈希表

*   std::set 和std::multiset 的底层实现是红黑树

    红黑树是一种平衡二叉搜索树，所以key值是有序的，但key不可以修改，改动key值会导致整棵树的错乱，所以只能删除和增加

##### 使用场景

*   std::unordered\_set：**优先使用，查询和增删效率是最优的**

*   std::set：集合有序

*   std::multiset：集合有序，且有重复数据

| 集合                  | 底层实现 | 是否有序 | 数值是否可以重复 | 能否更改数值 | 查询效率     | 增删效率     |
| :------------------ | :--- | :--- | :------- | :----- | :------- | :------- |
| std::set            | 红黑树  | 有序   | 否        | 否      | O(log n) | O(log n) |
| std::multiset       | 红黑树  | 有序   | 是        | 否      | O(logn)  | O(logn)  |
| std::unordered\_set | 哈希表  | 无序   | 否        | 否      | O(1)     | O(1)     |

*   虽然std::set、std::multiset 的底层实现是红黑树，不是哈希表，std::set、std::multiset 使用红黑树来索引和存储，不过给我们的使用方式，还是哈希法的使用方式，即key和value。所以使用这些数据结构来解决映射问题的方法，我们依然称之为哈希法

#### map（映射）

##### 底层实现

*   std::unordered\_map 底层实现为哈希表

*   std::map 和std::multimap 的底层实现是红黑树

    同理，std::map 和std::multimap 的key也是有序的（这个问题也经常作为面试题，考察对语言容器底层的理解）

| 映射                  | 底层实现 | 是否有序  | 数值是否可以重复 | 能否更改数值  | 查询效率     | 增删效率     |
| :------------------ | :--- | :---- | :------- | :------ | :------- | :------- |
| std::map            | 红黑树  | key有序 | key不可重复  | key不可修改 | O(logn)  | O(logn)  |
| std::multimap       | 红黑树  | key有序 | key可重复   | key不可修改 | O(log n) | O(log n) |
| std::unordered\_map | 哈希表  | key无序 | key不可重复  | key不可修改 | O(1)     | O(1)     |

### 总结

*   **当我们遇到了要快速判断一个元素是否出现集合里的时候，就要考虑哈希法**

*   但是哈希法是**牺牲了空间换取了时间**，因为我们要使用额外的数组，set或者是map来存放数据，才能实现快速的查找

*   如果在做面试题目的时候遇到需要判断一个元素是否出现过的场景也应该第一时间想到哈希法

## 有效的字母异位词

### 例题

[242. 有效的字母异位词 - 力扣（LeetCode）](https://leetcode.cn/problems/valid-anagram/description/)

##### 哈希表实现（数组）

```C++
class Solution
{
public:
   bool isAnagram(string s, string t)
   {
       int record[26] = {0}; // 创建一个长度为26的数组，用于记录字符出现的次数
       for (int i = 0; i < s.size(); i++)
       {
           // 遍历字符串s，对应字符增加计数
           record[s[i] - 'a']++;
       }
       for (int i = 0; i < t.size(); i++)
       {
           // 遍历字符串t，对应字符减少计数
           record[t[i] - 'a']--;
       }
       for (int i = 0; i < 26; i++)
       {
           // 如果数组中有非0元素，说明两个字符串不是字母异位词
           if (record[i] != 0)
           {
               return false;
           }
       }
       // 如果数组中所有元素都为0，说明两个字符串是字母异位词
       return true;
   }
};
```

*   时间复杂度: O(n)

*   空间复杂度: O(1)

## 查找常用字符

### 例题

[1002. 查找共用字符 - 力扣（LeetCode）](https://leetcode.cn/problems/find-common-characters/description/)

##### 哈希表实现

```C++
class Solution {
public:
    vector<string> commonChars(vector<string>& words) {
        vector<vector<int>> record(words.size(), vector<int>(26,0));
        for (int i = 0; i < words.size(); i++) {
            for (int j = 0; j < words[i].size(); j++) {
                record[i][words[i][j] - 'a']++;
            }
        }

        // //输出检查
        // for (int i = 0; i < words.size(); i++) { 
        //     for (int j = 0; j < 26; j++) {
        //         cout << record[i][j] << " ";
        //     }
        //     cout << endl;
        // }

        // 二维数组按列求最小值
        int hash[26];
        fill_n(hash, 26, INT32_MAX); // 定义hash为全INT32_MAX的26列数组
        for (int i = 0; i < 26; i++) {
            for (int j = 0; j < words.size(); j++) {
                if (hash[i] > record[j][i]) {
                    hash[i] = record[j][i];
                }
            }
        }

        // 结果输出
        vector<string> result;
        for (int k = 0; k < 26; k++) {
            while (hash[k] != 0)
            {
                string s(1, k + 'a');
                result.push_back(s);
                hash[k]--;
            }
        }
        return result;       
    }
};
```

## 两个数组的交集

### 例题

[349. 两个数组的交集 - 力扣（LeetCode）](https://leetcode.cn/problems/intersection-of-two-arrays/description/)

### **关键点**

*   **输出结果中的每个元素一定是唯一的**

    输出的结果的去重的， 同时可以不考虑输出结果的顺序

*   **使用数组来做哈希的题目，是因为题目都限制了数值的大小**

    而这道题目没有限制数值的大小，就无法使用数组来做哈希表了

    如果哈希值比较少、特别分散、跨度非常大，使用数组就造成空间的极大浪费

*   **std::unordered\_set的底层实现是哈希表**

    使用unordered\_set 读写效率是最高的，并不需要对数据进行排序，且不用让数据重复

##### unordered\_set实现

```C++
class Solution
{
public:
    // 定义一个函数，返回两个数组的交集
    vector<int> intersection(vector<int> &nums1, vector<int> &nums2)
    {
        // 创建一个无序集合，用于存储结果
        unordered_set<int> resultSet;
        // 创建一个无序集合，用于存储nums1中的元素
        unordered_set<int> numsSet(nums1.begin(), nums1.end());
        // 遍历nums2
        for (int num : nums2)
        {
            // 如果num在numsSet中，则将其添加到resultSet中
            if (numsSet.find(num) != numsSet.end())
            {
                resultSet.insert(num);
            }
        }
        // 返回结果集，将无序集合转换为向量
        return vector<int>(resultSet.begin(), resultSet.end());
    }
};
```

*   时间复杂度: O(n + m) m 是最后要把 set转成vector

*   空间复杂度: O(n)

## 快乐数

### 例题

[202. 快乐数 - 力扣（LeetCode）](https://leetcode.cn/problems/happy-number/description/)

### 关键点

*   求和的过程中，sum会重复出现，**无限循环**

*   **快速判断一个元素是否出现集合内**

*   **难点：求和的过程，对取数值各个位上的单数操作**

##### unordered\_set实现

```C++
class Solution
{
public:
   // 定义一个方法，用于计算一个数的每个位的平方和
   int getNum(int n)
   {
       int sum = 0;  // 初始化和为0
       while (n != 0)  // 当n不等于0时，循环计算
       {
           sum += pow((n % 10), 2);  // 计算n的最后一位数的平方，加到sum上
           n /= 10;  // 去掉n的最后一位数
       }
       return sum;  // 返回和
   }

   // 定义一个方法，用于判断一个数是否是快乐数
   bool isHappy(int n)
   {
       unordered_set<int> set;  // 定义一个无序集合，用于存储已经计算过的数
       while (1)  // 无限循环，直到满足条件退出
       {
           int sum = getNum(n);  // 计算n的每个位的平方和
           if (sum == 1)  // 如果和为1，则n是快乐数，返回true
           {
               return true;
           }
           if (set.find(sum) != set.end())  // 如果和已经在集合中，说明进入了循环，返回false
           {
               return false;
           }
           else  // 否则，将和插入集合，然后将n设为和，继续循环
           {
               set.insert(sum);
           }
           n = sum;
       }
   }
};
```

*   时间复杂度: O(logn)

*   空间复杂度: O(logn)
