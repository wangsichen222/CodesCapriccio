## 两数之和

### 例题

[1. 两数之和 - 力扣（LeetCode）](https://leetcode.cn/problems/two-sum/)

### 关键

*   不仅要知道元素有没有遍历过，还要知道这个元素对应的下标

*   需要使用 key value结构来存放，key来存元素，value来存下标，故**使用map**

*   map目的用来存放我们访问过的元素，遍历数组时需要记录遍历过哪些元素和对应的下标

*   判断元素是否出现：map中的存储结构为 {**key：数据元素**，**value：数组元素对应的下标**}



### 数组 与 set 的局限

*   数组的大小是受限制的，而且如果元素很少，而哈希值太大会造成内存空间的浪费

*   set是一个集合，里面放的元素只能是一个key，而两数之和这道题目，不仅要判断y是否存在而且还要记录y的下标位置，因为要返回x 和 y的下标。所以set 也不能用

##### 暴力解法

```C++
// 暴力解法
class Solution
{
public:
    vector<int> twoSum(vector<int> &nums, int target)
    {
        int k1, k2;
        for (int i = 0; i < nums.size(); i++)
        {
            for (int j = i + 1; j < nums.size(); j++)
            {
                if (nums[i] + nums[j] == target)
                {
                    k1 = i;
                    k2 = j;
                }
            }
        }
        return {k1, k2};
    }
};
```

##### 哈希解法（map）

```C++
// 哈希法
class Solution
{
public:
   // 定义函数twoSum，输入参数为一个整数数组和一个目标数，返回一个包含两个整数的vector
   vector<int> twoSum(vector<int> &nums, int target)
   {
       // 定义一个无序map，键为int类型，值为int类型
       unordered_map <int,int> map;

       // 遍历数组
       for (int i = 0; i < nums.size(); i++)
       {
           // 在map中查找当前元素的补数（目标数减去当前元素）
           auto iter = map.find(target - nums[i]); // 如果找到了，说明已经找到了一半，返回这两个数的索引
           if(iter != map.end()) {
               return {iter->second, i};
           }
           // 如果没找到，就把当前元素和它的索引加入到map中，继续寻找下一个元素的补数
           map.insert(pair<int, int>(nums[i], i)); 
       }
       // 如果遍历完都没找到，返回空vector
       return {};
   }
};
```

*   时间复杂度: O(n)

*   空间复杂度: O(n)

## 三数之和

### 例题

[15. 三数之和 - 力扣（LeetCode）](https://leetcode.cn/problems/3sum/description/)

##### 哈希解法（set）

*   两数之和的基础上，增加一层循环

```C++
class Solution
{
public:
   // 定义一个函数，输入一个整数数组，返回所有可能的三元组组合
   vector<vector<int>> threeSum(vector<int> &nums)
   {
       vector<vector<int>> result; // 定义结果数组
       sort(nums.begin(), nums.end()); // 对输入数组进行排序
       for (int i = 0; i < nums.size(); i++)
       {
           if (nums[i] > 0)
           { // 如果排序后的第一个元素已经大于零，那么不可能凑成三元组，提前退出循环
               break;
           }
           if (i > 0 && nums[i] == nums[i - 1])
           { // 如果元素a重复，跳过当前循环，进行下一次循环
               continue;
           }
           unordered_set<int> set; // 定义一个无序集合，用于存储元素j
           for (int j = i + 1; j < nums.size(); j++)
           {
               if (j > i + 2 && nums[j] == nums[j - 1] && nums[j - 1] == nums[j - 2])
               { // 如果元素b重复，跳过当前循环，进行下一次循环
                   continue;
               }
               int c = 0 - (nums[i] + nums[j]); // 计算元素c的值
               if (set.find(c) != set.end())
               { // 如果元素c在集合中存在，说明已经找到一个三元组，将其添加到结果数组中，并从集合中移除元素j
                   result.push_back({nums[i], nums[j], c});
                   set.erase(c); 
               }
               else
               {
                   set.insert(nums[j]); // 如果元素c不在集合中，将元素j添加到集合中
               }
           }
       }
       return result; // 返回结果数组
   }
};
```

*   时间复杂度: O(n^2)

*   空间复杂度: O(n)，额外的 set 开销

##### 双指针法

```C++
// 双指针法
class Solution {
public:
    vector<vector<int>> threeSum(vector<int> &nums)
    {
        vector<vector<int>> result;
        sort(nums.begin(), nums.end());
        for (int i = 0; i < nums.size(); i++)
        {

            if (nums[i] > 0)
            { // 排序之后如果第一个元素已经大于零，那么无论如何组合都不可能凑成三元组，直接返回结果就可以了
                return result;
            }
            // 错误去重a方法，将会漏掉-1,-1,2 这种情况
            /*
            if (nums[i] == nums[i + 1]) {
                continue;
            }
            */
            // 正确去重a方法
            if (i > 0 && nums[i] == nums[i - 1])
            {
                continue;
            }
            int left = i + 1;
            int right = nums.size() - 1;
            while (right > left)
            {
                // 去重复逻辑如果放在这里，0，0，0 的情况，可能直接导致 right<=left 了，从而漏掉了 0,0,0 这种三元组
                /*
                while (right > left && nums[right] == nums[right - 1]) right--;
                while (right > left && nums[left] == nums[left + 1]) left++;
                */
                if (nums[i] + nums[left] + nums[right] > 0)
                    right--;
                else if (nums[i] + nums[left] + nums[right] < 0)
                    left++;
                else
                {
                    result.push_back(vector<int>{nums[i], nums[left], nums[right]});
                    // 去重逻辑应该放在找到一个三元组之后，对b 和 c去重
                    while (right > left && nums[right] == nums[right - 1])
                        right--;
                    while (right > left && nums[left] == nums[left + 1])
                        left++;
                    // 找到答案时，双指针同时收缩
                    right--;
                    left++;
                }
            }
        }
        return result;
    }
};
```

*   时间复杂度: O(n^2)

*   空间复杂度: O(1)

*   两数之和 就不能使用双指针法，因为[1.两数之和](https://programmercarl.com/0001.%E4%B8%A4%E6%95%B0%E4%B9%8B%E5%92%8C.html)要求返回的是索引下标， 而双指针法一定要排序，一旦排序之后原数组的索引就被改变了。

*   如果[1.两数之和](https://programmercarl.com/0001.%E4%B8%A4%E6%95%B0%E4%B9%8B%E5%92%8C.html)要求返回的是数值的话，就可以使用双指针法了。

## 四数之和

### 例题1

[18. 四数之和 - 力扣（LeetCode）](https://leetcode.cn/problems/4sum/description/)

##### 双指针法

```C++
class Solution
{
public:
   // 使用四层循环，找出所有满足四数之和为target的组合
   vector<vector<int>> fourSum(vector<int> &nums, int target)
   {
       vector<vector<int>> result;  // 存储结果
       sort(nums.begin(), nums.end());  // 对输入数组进行排序
       for (int i = 0; i < nums.size(); i++)  // 外层循环
       {
           if (nums[i] > target && nums[i] >= 0)  // 如果当前数字大于目标值且大于等于0，则跳出循环
           {
               break;
           }
           if (i > 0 && nums[i] == nums[i - 1])  // 如果当前数字和上一个数字相同，则跳过当前循环
           {
               continue;
           }
           for (int j = i + 1; j < nums.size(); j++)  // 中层循环
           {
               if (nums[i] + nums[j] > target && nums[i] + nums[j] >= 0)  // 如果当前数字和下一个数字的和大于目标值且大于等于0，则跳出循环
               {
                   break;
               }
               if (j > i + 1 && nums[j] == nums[j - 1])  // 如果当前数字和上一个数字相同，则跳过当前循环
               {
                   continue;
               }
               int left = j + 1;  // 左指针
               int right = nums.size() - 1;  // 右指针
               while (right > left)  // 使用双指针在数组中寻找满足条件的组合
               {
                   if ((long) nums[i] + nums[j] + nums[left] + nums[right] > target)  // 如果当前组合和大于目标值，则移动右指针
                   {
                       right--;
                   }
                   else if ((long) nums[i] + nums[j] + nums[left] + nums[right] < target)  // 如果当前组合和小于目标值，则移动左指针
                   {
                       left++;
                   }
                   else  // 如果当前组合和等于目标值，则将其添加到结果中
                   {
                       result.push_back(vector<int>{nums[i], nums[j], nums[left], nums[right]});
                       while (right > left && nums[right] == nums[right - 1])  // 如果右指针指向的数字和其前一个数字相同，则移动右指针
                       {
                           right--;
                       }
                       while (right > left && nums[left] == nums[left + 1])  // 如果左指针指向的数字和其后一个数字相同，则移动左指针
                       {
                           left++;
                       }
                       left++;
                       right--;
                   }
               }
           }
       }
       return result;  // 返回结果
   }
};
```

*   时间复杂度: O(n^3)

*   空间复杂度: O(1)

### 例题2

[454. 四数相加 II - 力扣（LeetCode）](https://leetcode.cn/problems/4sum-ii/description/)

#### 解题步骤

*   首先定义 一个unordered\_map，key放a和b两数之和，value 放a和b两数之和出现的次数

*   遍历大A和大B数组，统计两个数组元素之和，和出现的次数，放到map中

*   定义int变量count，用来统计 a+b+c+d \= 0 出现的次数

*   在遍历大C和大D数组，找到如果 0-(c+d) 在map中出现过的话，就用count把map中key对应的value也就是出现次数统计出来

*   最后返回统计值 count 就可以了

##### 哈希解法（map）

```C++
class Solution
{
public:
    int fourSumCount(vector<int> &nums1, vector<int> &nums2, vector<int> &nums3, vector<int> &nums4)
    {
        int count = 0; // 统计a+b+c+d = 0 出现的次数
        unordered_map<int, int> umap; // key:a+b的数值，value:a+b数值出现的次数
        
        for (int a : nums1)
        {// 遍历大A和大B数组，统计两个数组元素之和，和出现的次数，放到map中
            for (int b : nums2)
            {
                umap[a + b]++;
            }
        }

        for (int c : nums3)
        { // 在遍历大C和大D数组，找到如果 0-(c+d) 在map中出现过的话，就把map中key对应的value也就是出现次数统计出来
            for (int d : nums4)
            {
                if (umap.find(0 - (c + d)) != umap.end())
                {
                    count += umap[0 - (c + d)];
                }
            }
        }
        return count;
    }
};
```

*   时间复杂度: O(n^2)

*   空间复杂度: O(n^2)，最坏情况下A和B的值各不相同，相加产生的数字个数为 n^2

## 赎金信

### 例题

[383. 赎金信 - 力扣（LeetCode）](https://leetcode.cn/problems/ransom-note/description/)

##### 暴力解法

```C++
class Solution
{
public:
   // 判断ransomNote是否可以由magazine构成
   bool canConstruct(string ransomNote, string magazine)
   {
       // 遍历magazine字符串
       for (int i = 0; i < magazine.length(); i++)
       {
           // 遍历ransomNote字符串
           for (int j = 0; j < ransomNote.length(); j++)
           {
               // 如果ransomNote的字符在magazine中，则删除该字符
               if (ransomNote[j] == magazine[i])
               {
                   ransomNote.erase(ransomNote.begin() + j);
                   break;
               }
           }
       }
       // 如果ransomNote为空，说明已经找齐所有字符，返回true
       if (ransomNote.length() == 0)
       {
           return true;
       }
       // 否则，返回false
       return false;
   }
};
```

*   时间复杂度: O(n^2)

*   空间复杂度: O(1)

##### 哈希解法（数组）

```C++
class Solution
{
public:
   // 判断ransomNote是否可以由magazine构成
   bool canConstruct(string ransomNote, string magazine)
   {
       // 记录magazine中每个字符出现的次数
       int record[26] = {0};
    //    // 如果ransomNote的长度大于magazine的长度，则无法构成
    //    if (ransomNote.size() > magazine.size())
    //    {
    //        return false;
    //    }
       // 遍历magazine，统计每个字符出现的次数
       for (int i = 0; i < magazine.length(); i++)
       {
           record[magazine[i] - 'a']++;
       }
       // 遍历ransomNote，检查每个字符在magazine中是否有足够的数量
       for (int j = 0; j < ransomNote.length(); j++)
       {
           // 对应字符的数量减一
           record[ransomNote[j] - 'a']--;
           // 如果小于0，说明ransomNote中的某个字符在magazine中不足，无法构成
           if (record[ransomNote[j] - 'a'] < 0)
           {
               return false;
           }
       }
       // 如果所有字符都能找到足够的数量，则返回true
       return true;
   }
};
```

*   时间复杂度: O(n)

*   空间复杂度: O(1)
