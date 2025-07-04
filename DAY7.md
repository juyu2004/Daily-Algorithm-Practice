# DAY - 7

## 颜色分类

https://leetcode.cn/problems/sort-colors/ - 颜色分类

```C++
class Solution 
{
public:
    void sortColors(vector<int>& nums) 
    {
        int left = 0, right = nums.size() - 1, i = 0;
        // 注意一个点, 就是 i <= right, 因为等于 right 这个地方可能还没有处理
        while(i <= right)
        {
            if(nums[i] == 0) swap(nums[i++], nums[left++]);
            else if(nums[i] == 1) i++;
            else swap(nums[i], nums[right--]);
        }
        return;
    }
};
```

https://leetcode.cn/problems/swap-nodes-in-pairs/solutions/3715387/san-ge-zhi-zhen-wan-cheng-liang-liang-ji-45oi/ - 两两交换链表中的节点

```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution 
{
public:
    ListNode* swapPairs(ListNode* head) 
    {
        if(head == nullptr || head->next == nullptr) return head;

        ListNode* newhead = new ListNode(0);
        ListNode* cur = head, *prev = newhead, *next = cur->next;
          
        while(cur != nullptr && next != nullptr)
        {
            ListNode* tmp = next->next;
            next->next = cur;
            cur->next = tmp;
            prev->next = next;

            prev = cur;
            cur = tmp;
            if(tmp) next = tmp->next;
        }

        cur = newhead->next;
        delete newhead;
        return cur;
    }
};
```

下面就是笔试强训 - DAY2 

https://www.nowcoder.com/practice/41b42e7b3c3547e3acf8e90c41d98270?tpId=290&tqId=39852&ru=/exam/oj - 牛牛的快递

版本1 : 利用了 ceil 函数对浮点数进行向上取整

```C++
#include <iostream>
#include <cmath>
using namespace std;

int main() {
    double x;
    char ch;
    while(cin >> x >> ch)
    {
        int ret = 0;
        if(ch == 'y') ret += 5;

        if(x <= 1) cout << ret + 20 << endl;
        else cout << ret + 20 + ceil(x - 1.0);
    }
    return 0;
}
```

扩展两个库函数 : ceil 和 floor (天花板和地板)

## 最小花费爬楼梯

https://www.nowcoder.com/practice/9b969a3ec20149e3b870b256ad40844e?tpId=230&tqId=39751&ru=/exam/oj - 最小花费怕楼梯

```C++
#include <iostream>
#include <vector>

using namespace std;

int main() 
{
    int n;
    cin >> n;
    
    vector<int> nums(n);
    for(int i = 0; i < n; i++) cin >> nums[i];

    // 创建 dp 表
    vector<int> dp(n);

    // 初始化 dp 表
    dp[0] = nums[0];
    // 注意 n == 1 的情况
    if(n == 1) 
    {
        cout << dp[0] << endl;
        return 0;
    }
    dp[1] = nums[1];

    // 填表
    for(int i = 2; i < n; i++)
        dp[i] = nums[i] + min(dp[i -1], dp[i - 2]);

    // 返回
    cout << min(dp[n - 1], dp[n - 2]) << endl;

    return 0;
}
// 64 位输出请用 printf("%lld")
```

https://www.nowcoder.com/questionTerminal/2c6a0a8e1d20492f92941400036e0890 - 数组中两个字符串的最小距离

```C++
#include <iostream>
#include <string>
#include <algorithm>

using namespace std;

int main() 
{
    int n;
    string str1, str2;
    cin >> n;
    cin >> str1 >> str2;

    int prev1 = -1, prev2 = -1, ret = 100010;
    for(int i = 0; i < n; ++i)
    {
        string s;
        cin >> s;
        if(s == str1)
        {
            if(prev2 != -1)
                ret = min(ret, i - prev2);
            prev1 = i;
        }

        if(s == str2)
        {
            if(prev1 != -1)
                ret = min(i - prev1, ret);
            prev2 = i;
        }
    }
    if(ret == 100010) cout << -1 << endl;
    else cout << ret << endl;
    return 0;
}
```

```C++
链接：https://www.nowcoder.com/questionTerminal/2c6a0a8e1d20492f92941400036e0890?toCommentId=21794190
来源：牛客网

#include <iostream>
#include <vector>
#include <string>

using namespace std;

int main()
{
    int n;
    cin >> n;
    cin.ignore();

    string str1, str2;
    cin >> str1 >> str2;

    vector<string> nums(n);
    for(int i = 0; i < n; i++)
        getline(cin, nums[i]);

    int flag1 = true, flag2 = true;
    for(auto c : nums)
    {
        if(str1 == c) flag1 = false;
        if(str2 == c) flag2 = false;
    }
    if(flag1 || flag2)
    {
        cout << -1 << endl;
        return 0;
    }

    int min_len = n + 1, current_len = 0;
    int pos1 = - 1, pos2 = -1;
    for(int i = 0; i < n; i++)
    {
        if(nums[i] == str1) pos1 = i;
        if(nums[i] == str2) pos2 = i;
        if(pos1 != -1 && pos2 != -1)
        {
            current_len = abs(pos1 - pos2);
            if(current_len < min_len) min_len = current_len;
        }
    }
    cout << min_len << endl;
    return 0;
   
}
```

这里运用了贪心, 只需要定一个数, 看看它前面有没有