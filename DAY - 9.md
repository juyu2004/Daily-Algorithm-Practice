# DAY - 9

## 无重复字符的最长字串

```C++
class Solution 
{
public:
    int lengthOfLongestSubstring(string s) 
    {
        int last_pos[256];
        // 初始化为 -1 表示尚未出现
        fill(last_pos, last_pos + 256, -1);  
        int left = 0, right = 0, max_len = 0;
        for(int right = 0; right < s.size(); right++)
        {
            char c = s[right];
            // 如果有重复的字符，应该更新为之前出现过的下一个位置
            if(last_pos[c] >= left) left = last_pos[c] + 1;
            // 跟新它的一个位置
            last_pos[c] = right;
            max_len = max(right - left + 1, max_len);
        }
        return max_len;
    }
};
```

## 从前序与中序遍历序列构造二叉树

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
class Solution 
{
    unordered_map<int, int> map;
    int preindex = 0;
public:
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) 
    {
        // 构建 value 到 index 的映射
        for(int i = 0; i < inorder.size(); i++)
            map[inorder[i]] = i;
        // 递归构建整个树, 初始范围是中序 [0, inorder.size() - 1]
        return build(preorder, inorder, 0, inorder.size() - 1);
    }
    TreeNode* build(vector<int>& preorder, vector<int>& inorder, int left, int right)
    {
        // 递归出口
        if(left > right) return nullptr;
        // 1. 取前序当前根节点
        int rootval = preorder[preindex];
        // 创建根节点
        TreeNode* root = new TreeNode(rootval);
        // 将我们前序索引后移，寻找下一个根节点
        preindex++;
        // 2. 找到根节点在中序的位置，分割左右子树
        int rootindex = map[rootval];
        // 3. 递归构建左右子树
        // 左子树中序范围 - [0, rootindex - 1]
        root->left = build(preorder, inorder, left, rootindex - 1);
        // 右子树的中序范围 - [rootindex + 1, right]
        root->right = build(preorder, inorder, rootindex + 1, right);
        return root;
    }
};
```

## **WY22 Fibonacci数列**

```C++
#include <iostream>
#include <vector>
using namespace std;

const int N = 1000000;

int main() 
{
    int n;
    cin >> n;
    vector<int> Fibonacci{1, 2};
    while(Fibonacci.back() < 2 * N)
    {
        int next = Fibonacci[Fibonacci.size() - 1] + Fibonacci[Fibonacci.size() - 2];
        Fibonacci.push_back(next);
    }

    for(int i = 0; i < n; i++)
    {
        // 这里一定要取到 >= ， 因为它可能会取到 Fibonacci 数组中的值，不然它就往后找了
        if(Fibonacci[i] >= n)
        {
            int diff1 = Fibonacci[i] - n;
            int diff2 = n - Fibonacci[i - 1];
            cout << min(diff1, diff2) << endl;
            break;
        }
    }

    return 0;

}
 
```

还有一种更简单的方法:

```C++
#include <iostream>
using namespace std;

int main() 
{
    int n;
    cin >> n;
    int a = 0, b = 1, c = 1;
    while(n > c)
    {
        a = b;
        b = c;
        c = a + b;
    }

    cout << min(c - n, n - b);

    return 0;
}
```

## 杨辉三角

```C++
#include <iostream>
#include <vector>

using namespace std;

int main() 
{
    int n;
    cin >> n;

    vector<vector<int>> ret;
    for(int i = 1; i <= n; i++)
    {
        vector<int> tmp(i, 1);
        ret.push_back(tmp);
    }

    for(int i = 2; i < n; i++)
    {
        for(int j = 1; j < ret[i].size() - 1; j++)
        {
            ret[i][j] = ret[i - 1][j - 1] + ret[i - 1][j];
        }
    }

    for(int i = 0; i < n; ++i)
    {
        for(int j = 0; j < ret[i].size(); j++)
            cout << ret[i][j] << " ";
        cout << endl;
    }

    return 0;
}
```

我这个笨的方法其实就是动态规划! (最简单的dp问题求解)

```C++
#include <iostream>
#include <vector>

using namespace std;

int dp[31][31];

int main() 
{
    int n;
    cin >> n;

    // 从 1 开始避免边界问题
    dp[1][1] = 1;

    for(int i = 2; i <= n; ++i)
        for(int j = 1; j <= i; ++j)
            dp[i][j] = dp[i - 1][j] + dp[i - 1][j - 1];

    for(int i = 1; i <= n; i++)
    {
        for(int j = 1; j <= i; j++)
            printf("%5d", dp[i][j]);
        cout << endl;
    }
    return 0;
}
```

## **NC242** **单词搜索**

```C++
class Solution {
public:
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param board string字符串vector 
     * @param word string字符串 
     * @return bool布尔型
     */
    bool vis[101][101] = {0};
    int m, n;
    int dx[4] = {0, 0, 1, -1};
    int dy[4] = {1, -1, 0, 0};

    bool exist(vector<string>& board, string word) 
    {
        m = board.size(), n = board[0].size();
        for(int i = 0; i < m; i++)
            for(int j = 0; j < n; j++)
                if(board[i][j] == word[0]) 
                    if(dfs(board, word, i, j, 0)) return true;
        return false;
    }

    bool dfs(vector<string>& board, string word, int i, int j, int pos)
    {
        if(pos == word.size() - 1) return true;

        vis[i][j] = true;
        for(int k = 0; k < 4; k++)
        {
            int a = i + dx[k], b = j + dy[k];
            if(a >= 0 && a < m && b >= 0 && b < n && !vis[a][b] && board[a][b] == word[pos + 1])
            {
                if(dfs(board, word, a, b, pos + 1)) return true;
            }
        }
        // 回溯
        vis[i][j] = false;

        return false;
    }
};
```

