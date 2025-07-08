# DAY - 12

## 字母异位词分组 - C++

```C++
#include <iostream>
#include <vector>
#include <unordered_map>
#include <string>
#include <algorithm>

using namespace std;

class Solution 
{
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) 
    {
        vector<vector<string>> ret;
        unordered_map<string, vector<string>>groups;
        for(auto x : strs)
        {
            string key = x; // 复制原字符用于排序
            sort(key.begin(), key.end()); // 排序生成 key
            groups[key].push_back(x); // 相同 key 的字符放入同一组
        }
        // 将哈希表的值转化为结果
        for(auto& pair : groups)
            ret.push_back(pair.second);

        return ret;
    }
};

作者：无关风月
链接：https://leetcode.cn/problems/sfvd7V/solutions/3718295/zi-mu-yi-wei-ci-fen-zu-c-by-wu-guan-feng-s7ch/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

## 两数之和 - C++

```C++
class Solution 
{
public:
    vector<int> twoSum(vector<int>& nums, int target) 
    {
        unordered_map<int, int> hash; // 值 -> 索引
        for(int i = 0; i < nums.size(); i++)
        {
            int tmp = target - nums[i];
            if(hash.count(tmp)) return {i, hash[tmp]};
            hash[nums[i]] = i;
        } 
        return {};
    }
};

作者：无关风月
链接：https://leetcode.cn/problems/two-sum/solutions/3718334/liang-shu-zhi-he-c-by-wu-guan-feng-yue-c-ymep/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

## 路径总和 III - 双重递归暴力枚举

```C++
// struct TreeNode
// {
//     int val;
//     TreeNode* _left;
//     TreeNode* _right;
//     TreeNode() : val(0), _left(nullptr), _right(nullptr) {}
//     TreeNode(int x) : val(x), _left(nullptr), _right(nullptr) {}
//     TreeNode(int x, TreeNode* left, TreeNode* right) : val(x), _left(left), _right(right){}
// };

class Solution 
{
public:
    int pathSum(TreeNode* root, int targetSum) 
    {
        if(!root) return 0;
        int count = dfs(root, targetSum, 0);
        count += pathSum(root->left, targetSum);
        count += pathSum(root->right, targetSum);

        return count;
    }

    int dfs(TreeNode* root, int targetSum, long currentSum)
    {
        if(!root) return 0;
        currentSum += root->val; // 加上当前节点
        int res = 0;
        if(currentSum == targetSum) res += 1; // 找到一天有效路径
        // 递归遍历左右子树, 继续延伸路径
        res += dfs(root->left, targetSum, currentSum);
        res += dfs(root->right, targetSum, currentSum);
        return res;
    }
};

作者：无关风月
链接：https://leetcode.cn/problems/path-sum-iii/solutions/3718380/lu-jing-zong-he-iii-shuang-zhong-di-gui-cu2c7/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

> 1 定义辅助函数 dfs(node, currentSum)：

> ​    从 node 出发，向下遍历子树，累计路径和 currentSum；

> ​    若 currentSum == targetSum，说明找到一条有效路径，计数 +1；

> ​    递归遍历 node->left 和 node->right，继续延伸路径。

> 2 主函数 pathSum(root, targetSum)：

> ​    若 root 为空，返回 0；

> ​    统计「以 root 为起点」的路径数（调用 dfs(root, 0)）；

> ​    递归统计「左子树所有起点」和「右子树所有起点」的路径数，三者相加即为结果。

## 腐烂的橘子

```C++
class Solution 
{
    int dx[4] = {0, 0, 1, -1};
    int dy[4] = {1, -1, 0, 0};
    int n, m;
public:
    int orangesRotting(vector<vector<int>>& grid) 
    {
        int freshCount = 0;
        n = grid.size(), m = grid[0].size();
        queue<pair<int, int>> q;
        for(int i = 0; i < n; i++)
        {
            for(int j = 0; j < m; j++)
            {
                if(grid[i][j] == 2) q.push({i, j});
                else if(grid[i][j] == 1) freshCount++;
            }
        }

        if(freshCount == 0) return 0;

        int count = -1;

        while(q.size())
        {
            int sz = q.size();
            count++;
            while(sz--)
            {
                auto [x, y] = q.front();
                q.pop();
                for(int k = 0; k < 4; k++)
                {
                    int a = x + dx[k];
                    int b = y + dy[k];

                    if(a >= 0 && a < n && b >= 0 && b < m && grid[a][b] == 1)
                    {
                        grid[a][b] = 2;
                        freshCount--;
                        q.push({a, b});
                    }
                }
            }
        }
        return freshCount == 0 ? count : -1;
    }
}; 

作者：无关风月
链接：https://leetcode.cn/problems/rotting-oranges/solutions/3718513/fu-lan-de-ju-zi-by-wu-guan-feng-yue-ca-vq54/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

## 单词搜索 - DFS

```C++
// DFS 核心思路
// 遍历矩阵找起点：先找出所有单词首字母的位置。
// 从每个起点出发 DFS：递归检查四个方向，确保路径上的字符与单词顺序一致。
// 回溯处理：在递归返回前，恢复当前位置的访问状态（标记为未访问），允许其他路径再次使用该位置。

#include <vector>
#include <queue>

using namespace std;

class Solution 
{
    bool vis[20][20] = {0};
    int dx[4] = {0, 0, 1, -1};
    int dy[4] = {1, -1, 0, 0};
    int n, m;
public:
    bool exist(vector<vector<char>>& board, string word) 
    {
        n = board.size(), m = board[0].size();
        for(int i = 0; i < n; i++)
        {
            for(int j = 0; j < m; j++)
            {
                if(board[i][j] == word[0])
                {
                    vis[i][j] = true;
                    if(dfs(board, word, i, j, 1)) return true;
                    vis[i][j] = false;
                }
            }
        } 
        return false;
    }

    bool dfs(vector<vector<char>>& board, string word, int i, int j, int idx)
    {
        if(idx == word.size()) return true;
        for(int k = 0; k < 4; k++)
        {
            int a = i + dx[k];
            int b = j + dy[k];
            if(a >= 0 && a < n && b >= 0 && b < m && !vis[a][b] && board[a][b] == word[idx])
            {
                vis[a][b] = true;
                if(dfs(board, word, a, b, idx + 1)) return true;
                vis[a][b] = false;
            }
        }
        return false;
    }
};

作者：无关风月
链接：https://leetcode.cn/problems/word-search/solutions/3718490/dan-ci-sou-suo-dfs-by-wu-guan-feng-yue-c-i9yi/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

