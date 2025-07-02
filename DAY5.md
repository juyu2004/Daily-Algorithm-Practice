# DAY5

## 递归搜索回溯 - 子集

![image-20250625122803400](C:\Users\35064\AppData\Roaming\Typora\typora-user-images\image-20250625122803400.png)

**算法思路:**

为了获得 nums 数组的所有子集, 我们需要对数组中的每个元素进行选择或不选择的操作, 即 nums 数组一定存在 2 ^(数组长度) 个子集. 对于查找子集, 具体可以定义一个数组, 来记录当前的状态, 并对其进行递归.

对于每个元素有两种选择: 1. 不进行任何操作; 2. 将其添加至当前状态的集合. 在递归时我们需要保证递归结束时当前状态与进行递归操作的状态不变, 而当我们在选择进行步骤2进行递归时, 当前状态会发生变化, 因此我们需要在递归结束时撤回添加操作, 即进行回溯.

解法一:

```C++
class Solution 
{
    vector<vector<int>> ret;
    vector<int> path;
public:
    vector<vector<int>> subsets(vector<int>& nums) 
    {
        dfs(nums, 0);
        return ret;
    }

    void dfs(vector<int>& nums, int pos)
    {
        if(pos == nums.size()) 
        {
            ret.push_back(path);
            return;
        }

        // 选
        path.push_back(nums[pos]);
        dfs(nums, pos + 1);
        path.pop_back();

        // 不选
        dfs(nums, pos + 1);
    }
};
```

解法二:

```C++
class Solution 
{
    vector<vector<int>> ret;
    vector<int> path;
public:
    vector<vector<int>> subsets(vector<int>& nums) 
    {
        dfs(nums, 0);
        return ret;
    }

    void dfs(vector<int>& nums, int pos)
    {
        ret.push_back(path);

        for(int i = pos; i < nums.size(); i++)
        {
            path.push_back(nums[i]);
            dfs(nums, i + 1);
            path.pop_back();
        }   
    }
};
```

## 动态规划 - 删除并获得点数

![image-20250625125549363](C:\Users\35064\AppData\Roaming\Typora\typora-user-images\image-20250625125549363.png)

算法思路:

其实这道题依旧是 [打家劫舍I] 问题的变型.

我们注意到题目描述, 选择 x 数字的时候, x - 1 与 x + 1 是不能被选择的. 像不像 [打家劫舍] 问题中, 选择 i 位置的金额后, 就不能选择 i - 1 位置以及 i + 1 位置的金额呢~

因此, 我们可以创建一个大小为 10001 (根据题目的数据范围) 的 hash 数组, 将 nums 数组中每一个元素 x, 累加到 hash 数组下标为 x 的位置处, 然后在 hash 数组上来一次 [打家劫舍]

1. 状态表示:

f[i] 表示:选到 i 位置的时候, nums[i] 必选, 此时能获得的最大点数

g[i] 表示: 选到 i 位置的时候, nums[i] 不选,此时能获得的最大点数

2. 状态转移方程:

f[i] = g[i - 1] + arr[i]

g[i] : 选择 i - 1 -> f[i - 1]

​	不选 i - 1 -> g[i - 1]

g[i] = max(f[i - 1], g[i -1])

```C++
class Solution 
{
public:
    int deleteAndEarn(vector<int>& nums) 
    {
        const int N = 10001;
        // 1. 预处理
        int arr[N] = {0};
        for(auto num : nums) arr[num] += num;

        // 2. 创建 dp 表
        vector<int> f(N);
        auto g = f;

        // 3. 填表
        for(int i = 1; i < N; i++)
        {
            f[i] = g[i - 1] + arr[i];
            // 其实就是 i 这个位置不选, 那么 i - 1 这个位置就可以选, 或者 i - 1 这个位置不选
            g[i] = max(g[i - 1], f[i - 1]);
        }
        // 返回
        return max(f[N - 1], g[N - 1]);
    }
};
```

