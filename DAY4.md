# DAY4

## 递归搜索回溯 - 全排列

给定一个不含重复数字的数组 nums , 返回其所有可能的全排列. 你可以 按任意顺序返回答案.

经典的回溯题目,我们需要在每一个位置上考虑所有的可能情况并且不能重复.通过深度优先搜索的方式,不断地枚举每个数在当前位置的可能性,并回溯到上一个状态,直到枚举完所有可能性,得到正确的结果.

每个数是否可以放入当前位置,只需要判断这个数在之前是否出现即可.具体地,在这道题目中,我们可以通过一个递归函数 backtrack 和标记数组 visited 来实现全排列.

递归函数设计:
void backtrack(vector<vector<int>& res, vector<int>& nums, vector<bool>^& visited, vector<int> &ans, int step, int len)

参数: step (当前需要填入的位置), len(数组长度)

返回值: 无;

函数作用: 查找所有合理的排列并存储在答案列表中.

递归流程如下:

1. 首先定义一个二维数组 res 用来存放所有可能的排列,一个一维数组 ans 用来存放每个状态的排列,一个一维数组 visited 标记元素,然后从第一个位置开始进行递归;
2. 在每个递归状态中, 我们维护一个步数 step, 表示当前已经处理了几个数字;
3. 递归结束条件: 当 step 等于 nums 数组的长度时, 说明我们已经处理完了所有数字, 将当前数组存入结果中;
4. 在每个递归状态中, 枚举所有下标 i, 若这个下标被标记, 则使用 nums 数组中当前下标的元素:
   1. 将 visited[i] 标记为 1;
   2. ans 数组中第 step 个元素被 nums[i] 覆盖;
   3. 对第 step + 1 个位置进行递归;
   4. 将 visited[i] 重新赋值为 0, 表示回溯;
5. 最后, 返回 res.

特别地, 我们可以不使用标记数组, 直接遍历 step 之后的元素 (未被使用), 然后将其与需要递归的位置进行交换即可.

```C++
class Solution {
    vector<vector<int>> ret;  // 存储所有排列的结果集
    vector<int> path;         // 记录当前递归路径上的排列
    bool check[10];           // 标记数组，check[i] 表示 nums[i] 是否已在当前路径中
    
public:
    vector<vector<int>> permute(vector<int>& nums) {
        // 初始化check数组为false
        memset(check, false, sizeof(check));
        // 从根节点开始深度优先搜索
        dfs(nums);
        return ret;
    }

    // 深度优先搜索函数，生成所有可能的排列
    void dfs(vector<int>& nums) {
        // 当路径长度等于数组长度时，说明已经生成了一个完整的排列
        if (path.size() == nums.size()) {
            ret.push_back(path);  // 将当前路径添加到结果集中
            return;
        }

        // 遍历数组中的每个数字
        for (int i = 0; i < nums.size(); i++) { // 递归的时候注意循环
            // 如果该数字未被使用
            if (!check[i]) {
                // 选择当前数字，加入路径
                path.push_back(nums[i]);
                check[i] = true;   // 标记为已使用
                
                // 递归生成剩余数字的排列
                dfs(nums);
                
                // 回溯：撤销选择，以便尝试其他可能性
                path.pop_back();   // 移除最后添加的数字
                check[i] = false;  // 取消标记
            }
        }
    }
};
```

这道题完全理解需要借助 for 这个关键循环, 比如, 你第一次 123 的时候, 回溯时, 3 进行释放, 那么在第二次递归处, 第二个位置可以选择3, 因为这是个 for 循环, i 这时候可以取 3 !!! 这是理解这道题目的关键!

## 动态规划 - 打家劫舍

算法思路:

先解决打家劫舍I:

![image-20250624180832201](C:\Users\35064\AppData\Roaming\Typora\typora-user-images\image-20250624180832201.png)算法思路(动态规划):

首先考虑最简单的情况. 如果只有一间房屋, 则偷窃该房屋到最高总金额. 如果只有两间房屋, 则由于两间房屋相邻, 不能同时偷窃, 只能偷窃其中的一间房屋, 因此选择其中金额较高的房屋进行偷窃, 可以偷窃到最高总金额.

如果房屋数量大于两间, 应该如何计算能够偷窃到的最高总金额呢? 对于第 k(k > 2) 间房屋, 有两个选项:

1. 偷窃第 k 间房屋, 那么就不能偷窃第 k - 1 间房屋, 偷窃总金额为 k - 2 间房屋的最高总金额与第 k 间房屋的总金额之和.
2. 不偷窃第 k 间房屋, 偷窃总金额为前 k - 1 间房屋的最高总金额

dp[i] 表示前 i 间房屋能偷窃到的最高总金额, 那么就有如下的状态转移方程:

dp[i] = max(dp[i - 2] + nums[i], dp[ i - 1])				 只有一件房屋, 则偷窃该房屋

边界条件 : dp[0] = nums[0] dp[1] = max(nums[0], nums[1]) 只有两间房屋, 选择其中金额较高的房屋进行偷窃

最终答案即为 dp[n - 1], 其中 n 是数组的长度.

```C++
class Solution {
public:
    int rob(vector<int>& nums) {
        // 创建 dp 表
        int n = nums.size();
        vector<int> dp(n);

        // 初始化 注意边界条件
        dp[0] = nums[0];
        if(n == 1) return dp[0];
        dp[1] = max(nums[0], nums[1]);
        if(n == 2) return dp[1];

        // 填表
        for(int i = 2; i < n; i++)
            dp[i] = max(dp[i - 2] + nums[i], dp[i - 1]);
        
        // 返回结果
        return dp[n - 1];
    }
};
```

现在回归我们之前的正题, 打家劫舍II :

![image-20250624182553341](C:\Users\35064\AppData\Roaming\Typora\typora-user-images\image-20250624182553341.png)

这个一个问题时 [打家劫舍I] 问题的变形.

上一个问题是一个 [单排] 的模式, 这一个问题是一个 [环形] 的模式, 也就是首位是相连的. 但是我们可以将 [环形] 问题转化为 [两个单排] 问题:

1. 偷第一个房屋时的最大金额 x , 此时不能偷最后一个房子, 因此就是偷 [0, n - 2] 区间的房子;
2. 不偷第一个房屋时的最大金额 y, 此时可以偷最后一个房子, 因此就是偷 [1, n - 1] 区间的房子

这两种情况下的 [最大值], 就是最终结果.

因此, 问题就转化为成求 [两次单排结果的最大值].

```C++
class Solution {
public:
    int rob(vector<int>& nums) {
        // 创建 dp 表
        int n = nums.size();
        if(n == 1) return nums[0];
        if(n == 2) return max(nums[0], nums[1]);

        // 选第一个, 那么区间范围就是 0 ~ n - 2
        vector<int> dp1(n);
        dp1[0] = nums[0];
        dp1[1] = max(nums[0], nums[1]);
        for(int i = 2; i < n - 1; i++)
            dp1[i] = max(dp1[i - 2] + nums[i], dp1[i - 1]);
        
        // 选第二个, 那么区间范围就是 1 ~ n - 1
        vector<int> dp2(n);
        dp2[1] = nums[1];
        for(int i = 2; i < n; i++)
            dp2[i] = max(dp2[i - 2] + nums[i], dp2[i - 1]);

        return max(dp1[n - 2], dp2[n - 1]);
    }
};
```

优化空间复杂度:

可以进一步优化空间复杂度, 将每个情况的 DP 数组压缩到常数空间:

```C++
class Solution {
public:
    int rob(vector<int>& nums) {
        // 创建 dp 表
        int n = nums.size();
        if(n == 1) return nums[0];
        if(n == 2) return max(nums[0], nums[1]);

        // 选第一个, 那么区间范围就是 0 ~ n - 2
        int prev1 = nums[0], cur1 = max(nums[0], nums[1]);
        for(int i = 2; i < n - 1; i++){
            int tmp = cur1;
            cur1 = max(cur1, prev1 + nums[i]);
            prev1 = tmp;
        }
        
        // 选第二个, 那么区间范围就是 1 ~ n - 1
        int prev2 = 0, cur2 = nums[1];
        for(int i = 2; i < n; i++){
            int tmp = cur2;
            cur2 = max(cur2, prev2 + nums[i]);
            prev2 = tmp;
        }
        return max(cur1, cur2);
    }
};
```

## 贪心 - 按身高排序

![image-20250624194403411](C:\Users\35064\AppData\Roaming\Typora\typora-user-images\image-20250624194403411.png)

**算法思路:** 

我们不能直接按照 i 位置对应的 heights 来排序, 因为排序过程会移动元素, 但是 names 内的元素是不会移动的.

由题意可知, names 数组和 heights 数组的下标是一一对应的, 因此我们可以重新创建出来一个下标数组, 将这个下标数组按照 heights[i] 的大小排序.

那么, 当下标数组排完序之后, 里面的顺序就相当于 heights 这个数组排完序之后的下标. 之后通过排序后的下标, 依次找到原来的 name, 完成名字的排序

```C++
class Solution {
public:
    vector<string> sortPeople(vector<string>& names, vector<int>& heights) {
        // 创建一个下标数组
        int n = names.size();
        vector<int> index(n);
        for(int i = 0; i < n; i++) index[i] = i;

        // 对 index 进行排序
        sort(index.begin(), index.end(), [&](int i, int j)
        {
            return heights[i] > heights[j];
        });

        vector<string> ret;
        for(int i : index)
            ret.push_back(names[i]);

        return ret;
    }
};
```

## 挑选旧题 - 最长连续序列

我想到了动态规划与贪心都可以做,但是我现在真的想不起来了(错了)

错的离谱!!! 最长递增子序列才是使用动态规划或者贪心去解决, 但是这是最长连续序列

这里我看了一下答案, 主要有两种方式, 第一种方式 : 哈希表 第二种方式 : 排序

![image-20250624201153710](C:\Users\35064\AppData\Roaming\Typora\typora-user-images\image-20250624201153710.png)

方式一 : 排序

经过我的尝试, 方式一 PASS 掉, 主要是去重方面实现不了

方式二: 哈希表

```C++
class Solution {
public:
    int longestConsecutive(vector<int>& nums) 
    {
        // 处理边界情况
        if(nums.size() == 0) return 0;

        unordered_set<int> hash;
        for(int i = 0; i < nums.size(); ++i)
            hash.insert(nums[i]);

        int  ret = 1;
        for(auto num : hash)
        {
            if(!hash.count(num - 1))
            {
                int curnum = num;
                int count = 1;

                while(hash.count(curnum + 1))
                {
                    curnum = curnum + 1;
                    count++;
                }
                ret = max(ret, count);
            }
        }
        return ret;
    }
};
```

做这道题需要注意: 

1. 首先用哈希表去重
2. 遍历哈希表: (这已经是去重结束后的数组),我们首先判断它前面一个数存在吗, 假如是100, 前面一个数就是99, 如果它前面一个数存在, 那么我们不需要理会这个数, 因为它肯定不会是以它的起始点的最长递增序列, 对于是起始点, 前面没有数据存在, 那我们就会遍历下去, 最后去最大即可. 

## 挑选新题 - 螺旋矩阵

![image-20250624204000539](C:\Users\35064\AppData\Roaming\Typora\typora-user-images\image-20250624204000539.png)

这道题同样两种解法: 

解法一: 模拟 (太复杂, 繁琐, 这里我就不写)

解法二: 按层模拟:
可以将矩阵看成若干层，首先输出最外层的元素，其次输出次外层的元素，直到输出最内层的元素。

定义矩阵的第 k 层是到最近边界距离为 k 的所有顶点。例如，下图矩阵最外层元素都是第 1 层，次外层元素都是第 2 层，剩下的元素都是第 3 层。

```C++
 [1, 1, 1, 1, 1, 1, 1],
 [1, 2, 2, 2, 2, 2, 1],
 [1, 2, 3, 3, 3, 2, 1],
 [1, 2, 2, 2, 2, 2, 1],
 [1, 1, 1, 1, 1, 1, 1]]

```

我的天啊, 这也太神奇了!!!
![image-20250624204909545](C:\Users\35064\AppData\Roaming\Typora\typora-user-images\image-20250624204909545.png)

```C++
class Solution 
{
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        if(matrix.size() == 0 || matrix[0].size() == 0) return {};

        int rows = matrix.size(), columns = matrix[0].size();
        vector<int> ret;
        int left = 0, right = columns - 1, top = 0, bottom = rows - 1;
        while(left <= right && top <= bottom)
        {
            for(int column = left; column <= right; column++) ret.push_back(matrix[top][column]);
            for(int row = top + 1; row <= bottom; row++) ret.push_back(matrix[row][right]);
            if(left < right && top < bottom)
            {
                for(int column = right - 1; column > left; column--) ret.push_back(matrix[bottom][column]);
                for(int row = bottom; row > top; row--) ret.push_back(matrix[row][left]);
            }
            left++;
            right--;
            top++;
            bottom--;
        }
        return ret;
    }
};
```

这道题最难的就是做到不乱!

```C++
class Solution 
{
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        // 处理空矩阵的情况
        if(matrix.size() == 0 || matrix[0].size() == 0) return {};

        // 获取矩阵的行数和列数
        int rows = matrix.size(), columns = matrix[0].size();
        vector<int> ret;  // 存储遍历结果的数组
        
        // 初始化四个边界变量：左、右、上、下
        int left = 0, right = columns - 1, top = 0, bottom = rows - 1;
        
        // 循环条件：当左边界小于等于右边界且上边界小于等于下边界时继续遍历
        while(left <= right && top <= bottom)
        {
            // 1. 从左到右遍历顶部行
            for(int column = left; column <= right; column++) 
                ret.push_back(matrix[top][column]);
            
            // 2. 从上到下遍历右侧列（从顶部下一行开始）
            for(int row = top + 1; row <= bottom; row++) 
                ret.push_back(matrix[row][right]);
            
            // 检查是否还有剩余元素需要遍历，避免重复处理单行或单列的情况
            if(left < right && top < bottom)
            {
                // 3. 从右到左遍历底部行（从右侧列前一列开始）
                for(int column = right - 1; column > left; column--) 
                    ret.push_back(matrix[bottom][column]);
                
                // 4. 从下到上遍历左侧列（从底部行开始）
                for(int row = bottom; row > top; row--) 
                    ret.push_back(matrix[row][left]);
            }
            
            // 缩小边界范围，向内移动一层
            left++;
            right--;
            top++;
            bottom--;
        }
        
        return ret;  // 返回遍历结果
    }
};
```

特别是这个 if 判断, 很容易漏掉!!!
