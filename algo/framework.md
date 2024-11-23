# 总论

种种数据结构，皆为**数组（顺序存储）**和**链表（链式存储）**的变换。

数据结构的关键点在于**遍历和访问**，即增删查改等基本操作。

种种算法，皆为**穷举**。

穷举的关键点在于**无遗漏和无冗余**。熟练掌握算法框架，可以做到无遗漏；充分利用信息，可以做到无冗余。

# 数据结构的存储方式
|数据结构|顺序存储|链式存储|
|--------------|------------------------------------------------|------------------------------------------------|
|队列或栈|数组，扩容缩容的问题|链表，更多的内存空间存储节点指针|
|哈希表|线性探查法，需要数组特性，以便连续寻址，不需要指针的存储空间|拉链法 需要链表特性，操作简单，但需要额外的空间存储指针|
|树结构|堆，完全二叉树，数组存储|普通树，每个节点存储子节点指针|
|图结构|邻接矩阵，二维数组存储|邻接表，每个节点存储相邻节点的指针|

# 数据结构的基本操作

对于任何数据结构，其基本操作无非**遍历 + 访问**，再具体一点就是：增删查改。

从最高层来看，各种数据结构的遍历 + 访问无非两种形式：线性的和非线性的。

线性就是 **for/while 迭代**为代表，非线性就是**递归**为代表。

比如，数组遍历框架：
```cpp
void traverse(vector<int>& arr) {
    for (int i = 0; i < arr.size(); i++) {
        // 迭代访问 arr[i]
    }
}
```
链表遍历框架：
```cpp
struct ListNode {
    int val;
    ListNode *next;
};
void traverse(ListNode *head) {
    for (ListNode *p = head; p != nullptr; p = p->next) {
        // 访问 p->val
    }
}
void traverse(ListNode *head) {
    // 递归访问 head->val
    traverse(head->next);
}
```
二叉树遍历框架：
```cpp
struct TreeNode {
    int val;
    TreeNode *left, *right;
};
void traverse(TreeNode *root){
    traverse(root->left);
    traverse(root->right);
}
```
N 叉树遍历框架：
```cpp
struct TreeNode {
    int val;
    vector<TreeNode*> children;
};
void traverse(TreeNode *root) {
    for (TreeNode *child : root->children)
        traverse(child);
}
```
# 算法的设计思想

算法的设计无非**穷举**，穷举的关键在于如何避免遗漏和重复。

遗漏会导致答案出错，冗余会拖慢算法的运行速度，导致超过时间限制。

为什么会遗漏？因为你对算法框架掌握不到位，不知道正确的穷举代码。

为什么会冗余？因为你没有充分利用信息。

所以，当你看到一道算法题，可以从这两个维度去思考：

1. 如何穷举？即无遗漏地穷举所有可能解。

2. 如何聪明地穷举？即避免所有冗余的计算，消耗尽可能少的资源求出答案。

不同类型的题目，难点是不同的，有的题目难在 **「如何穷举」**，有的题目难在 **「如何聪明地穷举」** 。

- 如何穷举类问题：

一般是**递归类问题**，比方说**回溯算法**、**动态规划**系列算法。
  
其中，回溯算法是 **「遍历」**的思维，而动态规划是 **「分解问题」**的思维。

穷举的核心是数学归纳法，明确函数的定义，分解问题，然后利用这个定义递归求解子问题。

- 如何聪明地穷举类问题：

一些耳熟能详的非递归算法技巧，都可以归在这一类。

最简单的例子，比方说让你在有序数组中寻找一个元素，用一个 for 循环暴力穷举谁都会，但 **二分搜索算法** 就是更聪明的穷举方式，拥有更好的时间复杂度。

还有后文 **Union Find** 并查集算法详解 告诉你一种高效计算连通分量的技巧，理论上说，想判断图中的两个节点是否连通，用 DFS/BFS 暴力搜索（穷举）肯定可以做到，但 Union Find 算法用数组模拟树结构，连通性相关的操作复杂度可以降到接近 O(1)。

**贪心算法**就是在题目中发现一些规律（专业点叫贪心选择性质），使得你不用完整穷举所有解就可以得出答案。

## 常见算法技巧

### 如何穷举

#### 二叉树系列算法

二叉树模型基本是所有高级算法的基础。尤其对应用递归的情况。

二叉树题目的递归解法可以分两类思路，第一类是**遍历**一遍二叉树得出答案，第二类是通过**分解问题**计算出答案，这两类思路分别对应着 
**回溯算法核心框架** 和 **动态规划核心框架**。

##### 遍历

一个普通的二叉树遍历问题：计算二叉树的最大深度。

```cpp
class Solution {
    public:
        //记录最大深度
        int res = 0;
        //记录当前深度
        int depth = 0;
        int maxDepth(TreeNode* root) {
            //递归遍历
            traverse(root);
            return res;
        }
        void traverse(TreeNode*) {
            if(root == nullptr) {
                res = max(res, depth);
                return;
            }
            //前序遍历位置
            depth++;
            traverse(root->left);
            traverse(root->right);
            //后序遍历位置  
            depth--;
        }
}
```

类似地，回溯算法中的全排列问题，也是通过遍历的方式得出答案。
```cpp
class Solution {
    public:
        //记录全排列
        vector<vector<int>> res;
        //记录当前路径
        vector<int> path;
        //记录当前路径中元素是否访问过
        vector<bool> used;
        //输入一组不重复的数字，返回它们的全排列
        vector<vector<int>> permute(vertor<int>& nums) {
            used = vector<bool>(nums.size(),false);
            backtracking(nums);
            return res;
        }
        //回溯算法核心框架，遍历回溯树，收集所有叶子节点上的全排列。
        void backtrack(vector<int>& nums) {
            //到达叶子节点
            if(path.size() == nums.size()) {
                res.push_back(path);
                return;
            }
            for(int i = 0; i < nums.size(); i++) {
                //排除使用过的
                if(used[i]) continue;
                //做选择
                path.push_back(nums[i]);
                used[i] = true;
                //递归
                backtrack(nums);
                //撤销选择
                path.pop_back();
                used[i] = false;
            }
        }
}
```
可以看出回溯本质是多叉树的遍历。

##### 分解问题

回到计算二叉树最大深度，可以使用**分治**写出如下解法。
```cpp
int maxDepth(TreeNode* root) {
    if(root == nullptr) return 0;
    //分治
    int left = maxDepth(root->left);
    int right = maxDepth(root->right);
    //合并结果
    return max(left, right) + 1;
}
```
上面代码的形式和动态规划解法类似。

对于使用动态规划算法的凑零钱问题：
> 有一堆面值不同的硬币，每种硬币的数量无限，给定一个金额，求最少需要多少硬币凑出这个金额。
```cpp
int coinChange(vector<int>& coins, int amount) {
    if(amount == 0) return 0;
    if(amount < 0) return -1;

    int res = INT_MAX;
    for(int coin : coins) {
        //子问题
        int subproblem = coinChange(coins, amount - coin);
        //子问题无解
        if(subproblem == -1) continue;
        //求最小值
        res = min(res, 1 + subproblem);
    }
    return res == INT_MAX ? -1 : res;
}
```

##### 思路拓展

对于二叉树的前序遍历，除了递归，还可以使用分解问题的思路来解决。

对于前序遍历的结果，可以发现，根节点总是在最前面，左子树在中间，右子树在最后。

所以，用分解问题的思路，可以写出如下解法。
```cpp
vector<int> preorderTraverse(TreeNode* root) {
    vector<int> res;
    if(root == nullptr) return res;
    //根节点在最前面
    res.push_back(root->val);
    //左子树在中间
    vector<int> left = preorderTraverse(root->left);
    res.insert(res.end(), left.begin(), left.end());
    //右子树在最后
    vector<int> right = preorderTraverse(root->right);
    res.insert(res.end(), right.begin(), right.end());
    return res;
}
```
其中，`res.insert(res.end(), left.begin(), left.end());`这一步，就是将左子树的结果插入到根节点的后面。

insert函数的第一个参数是插入的位置，第二个参数是插入的起始位置，第三个参数是插入的结束位置。

##### 层序遍历

BFS的核心代码就是根据二叉树的层序遍历框架写出来的。

```cpp
//输入二叉树的根节点，返回二叉树的层序遍历结果
int levelTraverse(TreeNode* root) {
    if(root == nullptr) return 0;
    queue<TreeNode*> q;
    q.push(root);
    int depth = 0;
    while(!q.empty()) {
        int size = q.size();
        //遍历当前层
        for(int i = 0; i < size; i++) {
            TreeNode* node = q.front();
            q.pop();
            if(node->left) q.push(node->left);
            if(node->right) q.push(node->right);
        }
        depth++;
    }
    return depth;
}
```
层序遍历的核心代码就是这样，只要稍作修改，就可以解决很多二叉树问题。

##### 二叉树总结

上述这些算法（遍历、分治、动态规划）本质都是穷举二叉树，有条件时通过剪枝或者备忘录减少冗余计算。








### 如何聪明地穷举
#### 数组/单链表系列算法

##### 双指针技巧

1. 快慢指针
2. 二分查找
3. 滑动窗口
4. 前缀和
5. 差分

