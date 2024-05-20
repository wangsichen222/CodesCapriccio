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

```C++
class Solution {
public:
    void traversal(TreeNode* cur, vector<int>& vec) {
        if (cur == NULL) return;
        vec.push_back(cur->val);    // 中
        traversal(cur->left, vec);  // 左
        traversal(cur->right, vec); // 右
    }
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> result;
        traversal(root, result);
        return result;
    }
};
```

##### 中序遍历

```C++
void traversal(TreeNode* cur, vector<int>& vec) {
    if (cur == NULL) return;
    traversal(cur->left, vec);  // 左
    vec.push_back(cur->val);    // 中
    traversal(cur->right, vec); // 右
}
```

##### 后续遍历

```C++
void traversal(TreeNode* cur, vector<int>& vec) {
    if (cur == NULL) return;
    traversal(cur->left, vec);  // 左
    traversal(cur->right, vec); // 右
    vec.push_back(cur->val);    // 中
}
```

### 例题1：前序遍历

[144. 二叉树的前序遍历 - 力扣（LeetCode）](https://leetcode.cn/problems/binary-tree-preorder-traversal/description/)

### 例题2：后序遍历

[145. 二叉树的后序遍历 - 力扣（LeetCode）](https://leetcode.cn/problems/binary-tree-postorder-traversal/description/)

### 例题3：中序遍历

[94. 二叉树的中序遍历 - 力扣（LeetCode）](https://leetcode.cn/problems/binary-tree-inorder-traversal/description/)
