# DAY - 8

## 简单写词

https://www.nowcoder.com/practice/0cfa856bf0d649b88f6260d878f35bb4?tpId=290&tqId=39937&ru=/exam/oj - 简单写词

```C++
#include <iostream>
#include <sstream>
#include <string>

using namespace std;

int main() 
{
    string str;
    // cin 遇到空格就会停止读取 需要用 getline 获取一整行字符串
    getline(cin, str);

    // 将一整行字符串以空格形式分割
    stringstream ss(str);

    string word, result;
    while(ss >> word)
    {
        result += toupper(word[0]);
    }

    cout << result << endl;
    return 0;
}
```

```C++
#include <iostream>
#include <sstream>
#include <string>

using namespace std;

int main() 
{
    string str;
    while(cin >> str) // cin 遇到空格停止
    {
        // 表达式 str[0] - 32 需要强转的
        if(str[0] >= 'a' && str[0] <= 'z') cout << char(str[0] - 32);
        else cout << str[0];
    }
    return 0;
}
```

## dd爱框框

https://ac.nowcoder.com/acm/problem/221681 - dd 爱框

```C++
#include <iostream>
#include <vector>

using namespace std;

int main()
{
    int n, x;
    cin >> n >> x;
    vector<int> nums(n);
    for(int i = 0; i < n; i++) cin >> nums[i];
    
    int left = 0, right = 0;
    int result_l = 0, result_r = 0;
    int min_len = n + 1;
    int current_sum = 0;
    
    while(right < n)
    {
        current_sum += nums[right++];
        while(current_sum >= x)
        {
            int len = right - left;
            if(len < min_len) 
            {
                min_len = len;
                // 目标是从 1 开始输入 !!!
                result_l = left + 1;
                result_r = right;
            }
            current_sum -= nums[left++];
        }
    }
    // 严谨一点, 如果没有找到 返回 0 0
    if(min_len == n + 1) cout << 0 << " " << 0 << endl;
    else cout << result_l << " " << result_r << endl;
    
    return 0;
}
```

## 除2!

```C++
#include <iostream>
#include <vector>
#include <queue>

using namespace std;

int main()
{
    int n, k;
    cin >> n >> k;
    vector<int> nums(n);
    for(int i = 0; i < n; i++) cin >> nums[i];
    
    priority_queue<int> heap;
    long total = 0;
    for(auto x : nums) 
    {
        if(x % 2 == 0) heap.push(x);
        total += x; 
    }
    
    while(!heap.empty() && k > 0)
    {
        int tmp = heap.top();
        heap.pop();
        total -= tmp / 2;
        if((tmp / 2) % 2 == 0) heap.push(tmp / 2);
        k--;
    }
    cout << total << endl;
    return 0;
}
```

## 合并区间

https://leetcode.cn/problems/merge-intervals/description/?envType=study-plan-v2&envId=top-100-liked - 合并区间

```C++
class Solution 
{
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) 
    {
        vector<vector<int>> ret;
        
        sort(intervals.begin(), intervals.end());
        ret.push_back(intervals[0]);

        for(int i = 0; i < intervals.size(); i++)
        {
            // 修改副本，不影响 ret 中的原始数据 - 需要加上引用
            vector<int>& last = ret.back();
            vector<int>& cur = intervals[i];

            if(last[1] >= cur[0]) last[1] = max(last[1], cur[1]);
            else ret.push_back(cur);
        }

        return ret;
    }
};
```

## 将有序数组转换为二叉搜索树

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
public:
    TreeNode* sortedArrayToBST(vector<int>& nums) 
    {
        return BuildBST(nums, 0, nums.size() - 1);
    }

    TreeNode* BuildBST(vector<int>& nums, int left, int right)
    {
        // 递归结束条件
        if(left > right) return nullptr;

        int mid = (left + right) / 2;
        TreeNode* root = new TreeNode(nums[mid]);

        root->left = BuildBST(nums, left, mid - 1);
        root->right = BuildBST(nums, mid + 1, right);

        return root;
    }
};
```

