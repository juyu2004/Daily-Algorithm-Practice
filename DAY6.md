# DAY6

旋转链表:

![image-20250702225439828](C:\Users\35064\AppData\Roaming\Typora\typora-user-images\image-20250702225439828.png)

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
    ListNode* rotateRight(ListNode* head, int k) 
    {
        if(head == nullptr || head->next == nullptr || k == 0) return head;

        int len = 0;
        ListNode* tail = head, *temp = head;
        while(tail != nullptr) 
        {
            temp = tail;
            tail = tail->next;
            len++;
        }    

        k %= len;
        int n = len - k - 1;
        temp->next = head;
        tail = head;
        while(n--)
            tail = tail->next;

        ListNode* newhead = tail->next;
        tail->next = nullptr;

        return newhead;
    }
};
```

![image-20250702230358132](C:\Users\35064\AppData\Roaming\Typora\typora-user-images\image-20250702230358132.png)

```C++
class Solution 
{
public:
    int minNumberOfFrogs(string croakOfFrogs) 
    {
        int current = 0, max_frogs = 0;
        int count[5] = {0};
        char map[256] = {0};

        map['c'] = 0, map['r'] = 1, map['o'] = 2, map['a'] = 3, map['k'] = 4;

        for(auto ch : croakOfFrogs)
        {
            int idx = map[ch];
            if(idx == 0)
            {
                current++;
                count[idx]++;
            }
            else if(idx == 4)
            {
                if(count[idx - 1] <= 0) return -1;
                count[idx -1]--;
                current--;
            }
            else
            {
                if(count[idx - 1] <= 0) return -1;
                count[idx - 1]--;
                count[idx]++;
            }

            max_frogs = max(max_frogs, current);
        }
        return current == 0 ? max_frogs : -1;
    }
};
```

![image-20250702231936683](C:\Users\35064\AppData\Roaming\Typora\typora-user-images\image-20250702231936683.png)

```C++
#include <iostream>
using namespace std;

int main() 
{
    int a, b;
    while (cin >> a >> b) 
    { 
        int count = 0;
        for(int i = a; i <= b; i++)
        {
            int n = i;
            while(n)
            {
                int temp = n % 10;
                if(temp == 2) count++;
                n /= 10;
            }
        }
        cout << count << endl;
    }
    return 0;
}
// 64 位输出请用 printf("%lld")
```

![image-20250702231958335](C:\Users\35064\AppData\Roaming\Typora\typora-user-images\image-20250702231958335.png)

```C++
class Solution {
public:
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param nums1 int整型vector 
     * @param nums2 int整型vector 
     * @return int整型vector
     */
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) 
    {
        vector<int> ret;

        unordered_set<int> hash1, hash2;
        for(auto x : nums1) hash1.insert(x);
        for(auto x : nums2) hash2.insert(x);

        for(int i = 1; i <= 1000; i++)
            if(hash1.count(i) && hash2.count(i)) ret.push_back(i);
        return ret;
    }
};
```

只用一个哈希表,而且是模拟哈希表:

```c++
class Solution {
public:
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param nums1 int整型vector 
     * @param nums2 int整型vector 
     * @return int整型vector
     */
    bool arr[1010] = {0};
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) 
    {
        vector<int> ret;

        for(auto x : nums1) arr[x] = true;
        for(auto x : nums2)
        {
            if(arr[x]) 
            {
                ret.push_back(x);
                arr[x] = false;
            }
        }
        return ret;
    }
};
```

![image-20250702233836664](C:\Users\35064\AppData\Roaming\Typora\typora-user-images\image-20250702233836664.png)

```C++
#include <iostream>
#include <string>
#include <stack>

using namespace std;

int main() 
{
    string s, ret;
    cin >> s;
    for(auto ch : s)
    {
        if(s.size() && ret.back() == ch) ret.pop_back();
        else ret += ch;
    }
     
    cout << (ret.size() == 0 ? "0" : ret) << endl;
    return 0;
}
```

个人版本: 笨重版:
```C++
#include <iostream>
#include <string>
#include <stack>
#include <algorithm>

using namespace std;

int main() 
{
    string s;
    cin >> s;
    stack<int> st;
    for(auto c : s)
    {
        if(st.size() && c == st.top()) st.pop();
        else st.push(c);
    }

    if(st.empty()) cout << "0" << endl;
    else 
    {
        string ret;
        while(!st.empty())
        {
            ret += st.top();
            st.pop();
        }
        reverse(ret.begin(), ret.end());
        cout << ret << endl;
    }
    return 0;

}
```

