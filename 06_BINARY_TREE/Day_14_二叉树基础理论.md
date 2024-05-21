## 二叉树基础理论

### 二叉树种类

*   **满二叉树**：如果一棵二叉树只有度为0的结点和度为2的结点，并且度为0的结点在同一层上，则这棵二叉树为满二叉树（深度为k，有2^k-1个节点的二叉树）

    ![](06_BINARY_TREE_md_files/a37b9720-0d08-11ef-b80c-3bf31077d146.jpeg?v=1\&type=image)

*   完全二叉树：除了最底层节点可能没填满外，其余每层节点数都达到最大值，并且最下面一层的节点都集中在该层最左边的若干位置。若最底层为第 h 层（h从1开始），则该层包含 1\~ 2^(h-1) 个节点

    ![](06_BINARY_TREE_md_files/933fde20-0d17-11ef-86b6-c12e963a9c91.jpeg?v=1\&type=image)

*   **二叉搜索树**：有数值，**二叉搜索树是一个有序树**

    *   若它的左子树不空，则左子树上所有结点的值均小于它的根结点的值；

    *   若它的右子树不空，则右子树上所有结点的值均大于它的根结点的值；

    *   它的左、右子树也分别为二叉排序树

    ![](06_BINARY_TREE_md_files/d6c99a00-0d17-11ef-86b6-c12e963a9c91.jpeg?v=1\&type=image)

*   **平衡二叉搜索树**：又被称为AVL（Adelson-Velsky and Landis）树，且具有以下性质：它是一棵空树或它的左右两个子树的高度差的绝对值不超过1，并且左右两个子树都是一棵平衡二叉树

    ![](06_BINARY_TREE_md_files/f4703d20-0d17-11ef-86b6-c12e963a9c91.jpeg?v=1\&type=image)

### 二叉树储存方式

**二叉树可以链式存储，也可以顺序存储。**

*   链式存储：指针，通过指针把分布在各个地址的节点串联一起

    ![](06_BINARY_TREE_md_files/1f0d3820-1331-11ef-bcb4-f3a72997bc9e.jpeg?v=1\&type=image)

*   顺序存储：数组，通过内存是连续分布

    **如果父节点的数组下标是 i，那么它的左孩子就是 i \* 2 + 1，右孩子就是 i \* 2 + 2**

    ![](06_BINARY_TREE_md_files/4ad08700-1331-11ef-bcb4-f3a72997bc9e.jpeg?v=1\&type=image)

### 二叉树遍历方式

两种遍历方式：

1.  **深度优先遍历（DFS）**：先往深走，遇到叶子节点再往回走，可以借助**栈**使用**递归的方式**来实现的

    *   前序遍历（递归法，迭代法）：中左右

    *   中序遍历（递归法，迭代法）：左中右

    *   后序遍历（递归法，迭代法）：左右中

    ![](06_BINARY_TREE_md_files/29e70100-1349-11ef-bcb4-f3a72997bc9e.jpeg?v=1\&type=image)

2.  **广度优先遍历（BFS）**：一层一层的去遍历，一般使用**队列**来实现，这也是队列先进先出的特点所决定的，因为需要先进先出的结构，才能一层一层的来遍历二叉树

    *   层次遍历（迭代法）

### 二叉树定义

```C++
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};
```

## 二叉树递归遍历

### 递归算法三要素

1.  **确定递归函数的参数和返回值：** 确定哪些参数是递归的过程中需要处理的，那么就在递归函数里加上这个参数， 并且还要明确每次递归的返回值是什么进而确定递归函数的返回类型。

2.  **确定终止条件：** 写完了递归算法, 运行的时候，经常会遇到栈溢出的错误，就是没写终止条件或者终止条件写的不对，操作系统也是用一个栈的结构来保存每一层递归的信息，如果递归没有终止，操作系统的内存栈必然就会溢出。

3.  **确定单层递归的逻辑：** 确定每一层递归需要处理的信息。在这里也就会重复调用自己来实现递归的过程‘

### 递归遍历实现

##### 前序遍历

[144. 二叉树的前序遍历 - 力扣（LeetCode）](https://leetcode.cn/problems/binary-tree-preorder-traversal/description/)

```C++
class Solution
{
public:
   // 前序遍历二叉树，将结点值添加到输入的vector中
   void traversal(TreeNode *cur, vector<int> &vec)
   {
       if (cur == NULL) // 如果当前节点为空，则直接返回
           return;
       vec.push_back(cur->val); // 将当前节点的值添加到vector中
       traversal(cur->left, vec); // 递归遍历左子树
       traversal(cur->right, vec); // 递归遍历右子树
   }

   // 对二叉树进行前序遍历，并返回遍历结果
   vector<int> preorderTraversal(TreeNode *root)
   {
       vector<int> result; // 用于存储遍历结果的vector
       traversal(root, result); // 对二叉树进行前序遍历
       return result; // 返回遍历结果
   }
};
```

##### 后序遍历

[145. 二叉树的后序遍历 - 力扣（LeetCode）](https://leetcode.cn/problems/binary-tree-postorder-traversal/description/)

```C++
class Solution {
public:
   // 后序遍历二叉树，将结果存储在vec中
   void traversal(TreeNode* cur, vector<int>& vec)
   {
       if (cur == NULL) // 如果当前节点为空，则直接返回
           return;
       traversal(cur->left, vec); // 先遍历左子树
       traversal(cur->right, vec); // 再遍历右子树
       vec.push_back(cur->val); // 将当前节点的值添加到vec中，这是后序遍历的特点
   }

   // 对二叉树进行后序遍历，返回遍历结果
   vector<int> postorderTraversal(TreeNode* root)
   {
       vector<int> result; // 存储遍历结果的vector
       traversal(root, result); // 调用traversal函数进行遍历
       return result; // 返回遍历结果
   }
};
```

##### 中续遍历

[94. 二叉树的中序遍历 - 力扣（LeetCode）](https://leetcode.cn/problems/binary-tree-inorder-traversal/description/)

```C++
class Solution
{
public:
   // 中序遍历二叉树的函数，使用递归实现
   void traversal(TreeNode *cur, vector<int> vec)
   {
       if (cur == NULL) // 如果当前节点为空，则直接返回
           return;
       traversal(cur->left, vec); // 先遍历左子树
       vec.push_back(cur->val); // 然后将当前节点的值添加到结果数组中
       traversal(cur->right, vec); // 最后遍历右子树
   }

   // 对外提供的中序遍历接口，接收一个二叉树的根节点，返回中序遍历的结果
   vector<int> inorderTraversal(TreeNode *root)
   {
       vector<int> vec; // 初始化结果数组
       traversal(root, vec); // 调用递归函数进行中序遍历
       return vec; // 返回遍历结果
   }
};
```

## 二叉树迭代遍历

用**栈**实现二叉树的前后中序遍历

### 迭代遍历实现

##### 前序遍历

*   前序遍历是**中左右**，每次先处理的是中间节点

*   先将**根节点**放入栈中，然后将**右孩子**加入栈，再加入**左孩子**

*   这样出栈的时候才是**中左右**的顺序

```C++
class Solution
{
public:
   // 使用栈实现二叉树的前序遍历（中-右-左）
   vector<int> preorderTraversal(TreeNode *root)
   {
       stack<TreeNode *> st; // 定义一个栈，用于存储节点
       vector<int> result; // 定义一个向量，用于存储遍历结果
     
       if (root == NULL) // 如果根节点为空，则直接返回空结果
           return result;
     
       st.push(root); // 将根节点压入栈中
       while (!st.empty()) // 当栈不为空时，继续遍历
       {
           TreeNode *node = st.top(); // 取栈顶节点
           st.pop(); // 弹出栈顶节点
           result.push_back(node->val); // 将栈顶节点的值添加到结果向量中
         
           if (node->right) // 如果右子节点存在
               st.push(node->right); // 将右子节点压入栈中（空节点不入栈）
           if (node->left) // 如果左子节点存在
               st.push(node->left); // 将左子节点压入栈中（空节点不入栈）
       }
     
       return result; // 返回遍历结果
   }
};

```

##### 后序遍历

```C++
class Solution
{
public:
   // 定义一个函数，用于实现二叉树的后序遍历
   vector<int> postorderTraversal(TreeNode *root)
   {
       stack<TreeNode *> st; // 定义一个栈，用于存储节点
       vector<int> result; // 定义一个向量，用于存储遍历结果
     
       if (root == NULL) // 如果根节点为空，则直接返回空结果
           return result;
     
       st.push(root); // 将根节点入栈
       while (!st.empty()) // 当栈不为空时，执行循环
       {
           TreeNode *node = st.top(); // 获取栈顶节点
           st.pop(); // 弹出栈顶节点
           result.push_back(node->val); // 将栈顶节点的值添加到结果向量中
         
           if (node->left) // 如果栈顶节点有左子节点
               st.push(node->left); // 将左子节点入栈 （空节点不入栈）
           if (node->right) // 如果栈顶节点有右子节点
               st.push(node->right); // 将右子节点入栈 （空节点不入栈）
       }
     
       reverse(result.begin(), result.end()); // 将结果向量反转，得到的就是后序遍历的结果
       return result; // 返回后序遍历结果
   }
};
```

##### 中序遍历

```C++
class Solution
{
public:
   // 定义一个函数，实现二叉树的中序遍历
   vector<int> inorderTraversal(TreeNode *root)
   {
       vector<int> result; // 存储中序遍历的结果
       stack<TreeNode *> st; // 定义一个栈，用于存储节点
       TreeNode *cur = root; // 初始化当前访问的节点为根节点
     
       while (cur != NULL || !st.empty()) // 当当前访问的节点不为空，或者栈不为空时，继续遍历
       {
           if (cur != NULL)
           {                    
               st.push(cur); // 如果当前访问的节点不为空，将其放入栈中
               cur = cur->left; // 然后访问其左子节点
           }
           else
           {
               cur = st.top(); // 如果当前访问的节点为空，说明已经访问到叶子节点，从栈中弹出一个节点
               st.pop();
               result.push_back(cur->val); // 将弹出的节点的值加入到结果数组中
               cur = cur->right; // 访问该节点的右子节点
           }
       }
       return result; // 返回中序遍历的结果
   }
};
```

### 统一迭代实现

**将访问的节点**放入栈中，把**要处理的节点**也放入栈

要处理的节点放入栈之后，紧接着放入一个**空指针**作为**标记**

##### 中序遍历

```C++
class Solution
{
public:
    vector<int> inorderTraversal(TreeNode *root)
    {
        vector<int> result;
        stack<TreeNode *> st;

        if (root != NULL)
            st.push(root);
        while (!st.empty())
        {
            TreeNode *node = st.top();
            if (node != NULL)
            {
                st.pop(); // 将该节点弹出，避免重复操作，下面再将右中左节点添加到栈中
                if (node->right)
                    st.push(node->right); // 添加右节点（空节点不入栈）

                st.push(node); // 添加中节点
                st.push(NULL); // 中节点访问过，但是还没有处理，加入空节点做为标记。

                if (node->left)
                    st.push(node->left); // 添加左节点（空节点不入栈）
            }
            else
            {                    // 只有遇到空节点的时候，才将下一个节点放进结果集
                st.pop();        // 将空节点弹出
                node = st.top(); // 重新取出栈中元素
                st.pop();
                result.push_back(node->val); // 加入到结果集
            }
        }
        return result;
    }
};
```

##### 前序遍历

```C++
class Solution
{
public:
    vector<int> preorderTraversal(TreeNode *root)
    {
        vector<int> result;
        stack<TreeNode *> st;

        if (root != NULL)
            st.push(root);
        while (!st.empty())
        {
            TreeNode *node = st.top();
            if (node != NULL)
            {
                st.pop();
                if (node->right)
                    st.push(node->right); // 右
                if (node->left)
                    st.push(node->left); // 左
                st.push(node);           // 中
                st.push(NULL);
            }
            else
            {
                st.pop();
                node = st.top();
                st.pop();
                result.push_back(node->val);
            }
        }
        return result;
    }
};

```

##### 后序遍历

    class Solution
    {
    public:
        vector<int> postorderTraversal(TreeNode *root)
        {
            vector<int> result;
            stack<TreeNode *> st;
            if (root != NULL)
                st.push(root);
            while (!st.empty())
            {
                TreeNode *node = st.top();
                if (node != NULL)
                {
                    st.pop();
                    st.push(node); // 中
                    st.push(NULL);

                    if (node->right)
                        st.push(node->right); // 右
                    if (node->left)
                        st.push(node->left); // 左
                }
                else
                {
                    st.pop();
                    node = st.top();
                    st.pop();
                    result.push_back(node->val);
                }
            }
            return result;
        }
    };

