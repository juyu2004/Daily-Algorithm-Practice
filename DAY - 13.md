# DAY - 13

## 反转链表

```C++
// 使用头插法进行反转
class Solution 
{
public:
    ListNode* reverseList(ListNode* head) 
    {
        ListNode* newhead = new ListNode(0);
        ListNode* cur = head;
        while(cur)
        {
            ListNode* tmp = cur->next;
            cur->next = newhead->next;
            newhead->next = cur;
            cur = tmp;
        }
        cur = newhead->next;
        delete newhead;
        return cur;
    }
};

// 递归进行反转
class Solution 
{
public:
    ListNode* reverseList(ListNode* head) 
    {
        if(head == nullptr || head->next == nullptr) return nullptr;
        
        ListNode* newhead = reverseList(head);
        head->next->next = head;
        head->next = nullptr;

        return newhead;
    }
};

作者：无关风月
链接：https://leetcode.cn/problems/reverse-linked-list/solutions/3719061/fan-zhuan-lian-biao-by-wu-guan-feng-yue-nwluf/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

## 相交链表

```C++
struct ListNode
{
    int val;
    ListNode* next;
    ListNode(int x) : val(x), next
}

class Solution 
{
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) 
    {
        ListNode* cur1 = headA, *cur2 = headB;
        int len1 = 0, len2 = 0;
        while(cur1)
        {
            len1++;
            cur1 = cur1->next;
        }
        while(cur2)
        {
            len2++;
            cur2 = cur2->next;
        }

        cur1 = headA, cur2 = headB;
        int k = abs(len1 - len2);
        if(len1 > len2) while(k--) cur1 = cur1->next;
        else while(k--) cur2 = cur2->next;

        while(cur1 && cur2)
        {
            if(cur1 == cur2) return cur1;
            cur1 = cur1->next;
            cur2 = cur2->next;
        } 

        return nullptr;
    }
};


作者：无关风月
链接：https://leetcode.cn/problems/intersection-of-two-linked-lists/solutions/3719041/xiang-jiao-lian-biao-by-wu-guan-feng-yue-x0kz/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution 
{
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) 
    {
        if(headA == nullptr || headB == nullptr) return nullptr;
        ListNode* cur1 = headA, *cur2 = headB;

        while(cur1 != cur2)
        {
            cur1 = cur1 ? cur1 = cur1->next : cur1 = headB;
            cur2 = cur2 ? cur2 = cur2->next : cur2 = headA;
        }
        return cur1;
    }
};

作者：无关风月
链接：https://leetcode.cn/problems/intersection-of-two-linked-lists/solutions/3719041/xiang-jiao-lian-biao-by-wu-guan-feng-yue-x0kz/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

使用两个指针pA和pB分别从链表 A 和链表 B 的头节点出发，同步遍历。当其中一个指针到达链表尾部时，将其重定向到另一个链表的头节点。这样，两个指针在第二次遍历时必然会在交点处相遇（如果存在交点），否则会同时到达链表尾部（nullptr）。

## 岛屿的最大面积

* 思路:

1. 遍历网格：逐个检查每个格子，若遇到陆地（1）且未被访问过（vis 数组标记），则启动 DFS。
2. DFS 计算机面积：从当前格子出发，递归遍历四个方向的相邻格子，累加陆地面积。
3. 标记已访问：用 vis 数组避免重复计算同一个格子。
4. 跟新最大值：每次 DFS 结束后，更新全局最大面积。

```C++
class Solution {
    int dx[4] = {0, 0, 1, -1};
    int dy[4] = {1, -1, 0, 0};
    bool vis[51][51] = {false};
    int n, m;
public:
    int maxAreaOfIsland(vector<vector<int>>& grid) {
        // 初始化类成员变量n和m
        n = grid.size();
        m = grid[0].size();
        int count = 0;
        
        for(int i = 0; i < n; i++) {
            for(int j = 0; j < m; j++) {
                if(grid[i][j] == 1 && !vis[i][j]) {
                    vis[i][j] = true;
                    count = max(dfs(grid, i, j, 1), count);
                }
            }
        }
        return count;
    }

    int dfs(vector<vector<int>>& grid, int x, int y, int sum) {
        // 遍历四个方向
        for(int k = 0; k < 4; k++) {
            int a = x + dx[k];
            int b = y + dy[k];
            if(a >= 0 && a < n && b >= 0 && b < m && !vis[a][b] && grid[a][b] == 1) {
                vis[a][b] = true;
                // 递归调用并累加返回值
                sum += dfs(grid, a, b, 1);
            }
        }
        return sum;
    }
};

作者：无关风月
链接：https://leetcode.cn/problems/ZL6zAn/solutions/3719124/dao-yu-de-zui-da-mian-ji-by-wu-guan-feng-w26d/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

## **NC109** **岛屿数量**

https://www.nowcoder.com/practice/0c9664d1554e466aa107d899418e814e?tpId=196&tqId=37167&ru=/exam/oj - 岛屿的数量

```C++
#include <iostream>
#include <vector>

using namespace std;

class Solution 
{
    bool vis[201][201] = {false};
    int n, m;
    int dx[4] = {0, 0, 1, -1};
    int dy[4] = {1, -1, 0, 0};
public:
    int solve(vector<vector<char> >& grid) 
    {
        if(grid.empty()) return 0; // 处理空网格
        
        int count = 0;
        n = grid.size(), m = grid[0].size();
        
        for(int i = 0; i < n; i++)
            for(int j = 0; j < m; j++)
                if(grid[i][j] == '1' && !vis[i][j])
                {
                    vis[i][j] = true;
                    dfs(grid, i, j);
                    count++;
                }
        return count;
    }

    void dfs(vector<vector<char>>& grid, int x, int y)
    {
        for(int k = 0; k < 4; k++)
        {
            int a = x + dx[k];
            int b = y + dy[k];
            if(a >= 0 && a < n && b >= 0 && b < m && !vis[a][b] && grid[a][b] == '1')
            {
                vis[a][b] = true;
                dfs(grid, a, b);
            }
        }
    }
};
```

## **REAL503** **字符串中找出连续最长的数字串**

https://www.nowcoder.com/practice/bd891093881d4ddf9e56e7cc8416562d?tpId=182&tqId=34785&ru=/exam/oj - 字符串中找出连续最长的数字串

```C++
#include <iostream>
#include <string>
#include <algorithm>

using namespace std;

int main() 
{
    string str;
    cin >> str;
    
    int left = 0, right = 0;
    int max_len = 0;
    // 记录最长数字串的起始索引和长度
    int start = 0, result_len = 0;

    while(right < str.size())
    {
        // 如果当前字母不是数字, 重置左边界
        if(!isdigit(str[right])) left = right + 1;
        // 当前窗口长度
        int current_len = right - left + 1;
        // 更新最长数字串
        if(current_len > result_len)
        {
            result_len = current_len;
            start = left; // 记录起始位置
        }
        right++;
    }
    cout << str.substr(start, result_len) << endl;
    return 0;
}

```

这道题的核心思路是 **用滑动窗口（双指针）遍历字符串，动态维护连续数字子串的区间，同时记录最长数字子串的起始位置和长度**，具体分以下几步：

1. **滑动窗口初始化**：用 `left` 和 `right` 两个指针表示当前窗口的左右边界，初始都指向字符串起始位置。
2. 遍历与窗口调整：
   - 右指针 `right` 逐个字符遍历字符串。
   - 若遇到**非数字字符**，就把左指针 `left` 直接跳到 `right + 1`（让窗口重新从下一个位置开始找连续数字）。
   - 若遇到**数字字符**，窗口自然向右扩展（`right` 继续右移，`left` 保持不动）。
3. **记录最长子串**：每次窗口变化后（`right` 移动），计算当前窗口长度 `right - left + 1`。若当前长度大于历史最长长度，就更新 “最长长度” 和 “最长子串的起始位置”。
4. **截取并输出结果**：遍历结束后，用 `substr(起始位置, 最长长度)` 截取字符串，输出最长连续数字子串。

## 拼三角

https://ac.nowcoder.com/acm/problem/219046 - 拼三角

算法思路： 简单枚举，不过有很多种枚举⽅法，我们这⾥之间⽤简单粗暴的枚举⽅式。

```C++
#include <iostream>
#include <algorithm>

using namespace std;

int t;
int arr[6];

int main()
{
    cin >> t;
    while(t--)
    {
        for(int i = 0; i < 6; i++) cin >> arr[i];
        sort(arr, arr + 6);
        if(arr[0] + arr[1] > arr[2] && arr[3] + arr[4] > arr[5] || 
           arr[0] + arr[2] > arr[3] && arr[1] + arr[4] > arr[5] ||
           arr[0] + arr[3] > arr[4] && arr[1] + arr[2] > arr[5] ||
           arr[0] + arr[4] > arr[5] && arr[1] + arr[2] > arr[3])
            cout << "Yes" << endl;
        else cout << "No" << endl;
    }
    return 0;
 }
```

