# DAY2

### 二叉树的所有路径

* 解法：（回溯）

算法思路：

使用深度优先遍历（DFS）求解。

路径以字符串形式存储，从根节点开始遍历，每次遍历时将当前节点的值加入到路径中，如果该节点为叶子节点，将路径存储到结果中。否则，将“->”加入到路径中并递归遍历该节点的左右子树。

定义一个结果数组，进行递归。递归具体实现方法如下：

1. 如果当前节点不为空，就将当前节点的值加入路径path中，否则直接返回；
2. 判断当前节点是否为叶子节点，如果是，则将当前路径加入到所有的路径的存储数组paths中；
3. 否则，将当前节点值加上“->”作为路径的分隔符，继续递归遍历当前节点的左右节点。
4. 返回结果数组。

特别的，我们可以只使用一个字符串存储每个状态的字符串，在递归回溯的过程中，需要将路径中的当前节点移除，以回到上一个节点。

具体实现方法如下：

1. 定义一个结果数组和一个路径数组

2. 从根节点开始递归，递归函数的参数为当前节点，结果数组和路径数组。

   a. 如果当前节点为空，返回。

   b. 将当前节点的值加入到路径数组中。

   c. 如果当前节点为叶子节点，将路径数组中的所有元素拼接成字符串，并将该字符串存储到结果数组中。

   d. 递归遍历当前节点的左子树

   e. 递归遍历的当前节点的右子树

   f. 回溯，将路径数组中的最后一个元素移除，以返回到上一个节点。

3. 返回数组结果

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
    vector<string> ret;
public:
    vector<string> binaryTreePaths(TreeNode* root) {
        string path;
        if(root == nullptr) return ret;
        dfs(root, path);
        return ret;
    }
    void dfs(TreeNode* root, string path)
    {
        if(root == nullptr) return;
        path += to_string(root->val);
        if(root->left == nullptr && root->right == nullptr)
        {
            ret.push_back(path);
            return;
        } 
        path += "->";
        dfs(root->left, path);
        dfs(root->right, path);
    }
};
```

### k次取反后最大化的数组和

贪心策略：

分情况讨论，设整个数组中负数的个数为m个：

1. m > k: 前k小负数，全部变成正数；
2. m == k：把所有的负数全部转化成正数；
3. m < k:
   1. 先把所有的负数变成正数；
   2. 然后根据k - m的奇偶分情况讨论：
      1. 如果是偶数，直接忽略
      2. 如果是奇数，挑选当前数组中最小的数，变成负数

```C++
class Solution {
public:
    int largestSumAfterKNegations(vector<int>& nums, int k) {
        int m = 0, minElem = INT_MAX, n = nums.size(), ret = 0;
        // m:数组中负数的个数，minElem:数组中的最小值，ret:最后返回的结果
        for(auto& num : nums)
        {
            if(num < 0) m++;
            minElem = min(minElem, abs(num));
        }
        if(m > k)
        {
            sort(nums.begin(), nums.end());
            for(int i = 0; i < k; i++) ret += -nums[i];
            for(int i = k; i < n; i++) ret += nums[i];
        }
        else
        {
            for(int i = 0; i < n; i++) ret += abs(nums[i]);
            if((k - m) % 2) ret -= 2 * minElem;
        }
        return ret;
    }
};
```

当你的贪心策略没有问题的时候，代码真的简单啊~

### 面试题17.16.按摩师

* 解法动态规划：

算法思路：

1. 状态表示：

对于简单的线性dp，我们可以用【经验 + 题目要求】来定义状态表示：

1. 以某个位置为结尾，巴拉巴拉；
2. 以某个位置为起点，巴拉巴拉；

这里我们选择比较常用的方式，以某个位置为结尾，结合题目要求，定义一个状态表示：
dp[i] 表示：选择到i位置时，此时的最长预约时长。

但是我们这个题在i位置的时候，会面临【选择】或者【不选择】两种抉择，所依赖的状态需要细分：
f[i]表示：选择到i位置时，nums[i]必选，此时的最长预约时长；

g[i]表示：选择到i位置时，nums[i]不选，此时的最长预约时长。

2. 状态转移方程

因为状态表示定义了两个，因此我们的状态转移方程也要分析两个：
对于f[i]:
如果nums[i]必选，那么我们仅需要知道i - 1位置在不选的情况下的最长预约时长，然后加上nums[i]即可，因此f[i] = g[i - 1] + nums[i]。

对于g[i]:

如果nums[i]不选，那么i - 1位置上选或者不选都可以。因此，我们需要知道i - 1位置上选或者不选都可以。因此，我们需要知道i - 1位置上选或者不选两种情况的最长时长，因此g[i] = max(f[i -1], g[i - 1])。

3. 初始化：

这道题的初始化比较简单，因此无需加辅助节点，仅需初始化f[0] = nums[0], g[0] = 0即可。

4. 填表顺序

根据【状态转移方程】得【从左往右，两个表一起填】。

5. 返回值

根据【状态表示】，应该返回max(f[n - 1], g[n - 1])。

```C++
class Solution {
public:
    int massage(vector<int>& nums) {
        // 创建dp表
        // 初始化
        // 填表
        // 返回值
        int n = nums.size();
        // 处理边界情况
        if(n == 0) return 0;
        // 创建dp表
        vector<int> f(n), g(n);
        // 初始化
        f[0] = nums[0], g[0] = 0;
        // 填表
        for(int i = 1; i < n; i++)
        {
            f[i] = g[i - 1] + nums[i];
            g[i] = max(g[i - 1], f[i - 1]);
        }
        // 返回结果
        return max(f[n - 1], g[n - 1]);
    }
};
```

