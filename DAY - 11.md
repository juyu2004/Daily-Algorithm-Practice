# DAY - 11

## 库存管理 IIII

https://leetcode.cn/problems/zui-xiao-de-kge-shu-lcof/description/ - 库存管理III

```C++
class Solution 
{
public:
    vector<int> inventoryManagement(vector<int>& stock, int cnt) 
    {
        vector<int> ret;
        sort(stock.begin(), stock.end());
        for(int i = 0; i < cnt; i++) ret.push_back(stock[i]);
        return ret;
    }
};
```

## 库存管理 II

https://leetcode.cn/problems/zui-xiao-de-kge-shu-lcof/ - 库存管理II

```C++
class Solution 
{
public:
    int inventoryManagement(vector<int>& stock) 
    {
        int tmp = stock.size() / 2;
        unordered_map<int, int> map;
        for(auto x : stock) 
        {
            map[x]++;
            if(map[x] > tmp) return x;
        }
        return -1;
    }
};
```

## 库存管理 I

https://leetcode.cn/problems/xuan-zhuan-shu-zu-de-zui-xiao-shu-zi-lcof/description/ - 库存管理I

```C++
class Solution 
{
public:
    int inventoryManagement(vector<int>& stock) 
    {
        int left = 0, right = stock.size() - 1;
        while(left < right)
        {
            int mid = left + (right - left) / 2;
            if(stock[mid] < stock[right]) right = mid;
            else if(stock[mid] > stock[right]) left = mid + 1;
            else right--;
        }    
        return stock[left];
    }
};
```

## 大数加法

```C++
class Solution 
{
public:
    string solve(string s, string t) 
    {
        if(s == "") return t;
        if(t == "") return s;

        string ret;

        int n = s.size() - 1, m = t.size() - 1;
        int idx = 0;

        while(n >= 0 || m >= 0 || idx)
        {
            if(n >= 0) idx += s[n--] - '0';
            if(m >= 0) idx += t[m--] - '0';
            ret += '0' + idx % 10;
            idx /= 10; 
        }

        reverse(ret.begin(), ret.end());
        return ret;
    }
};
```

## 大数乘法

https://www.nowcoder.com/practice/6b668316f0ac4421ae86d7ead4301b42 - 大数乘法

```C++
#include <iostream>
#include <string>
#include <algorithm>
#include <vector>

using namespace std;

int main() 
{
    string str1, str2;
    cin >> str1 >> str2;

    reverse(str1.begin(), str1.end());
    reverse(str2.begin(), str2.end());

    int n = str1.size(), m = str2.size();

    // 无进位乘法
    vector<int> tmp(m + n);
    for(int i = 0; i < n; i++)
        for(int j = 0; j < m; j++)
            tmp[i + j] += (str1[i] - '0') * (str2[j] - '0'); 

    string ret;
    int idx = 0;
    for(auto x : tmp)
    {
        idx += x;
        ret += idx % 10 + '0';
        idx /= 10;
    }

    while(idx)
    {
        ret += (idx % 10) + '0';
        idx /= 10;
    }

    // 去除前缀 0
    while(ret.size() > 1 && ret.back() == '0') ret.pop_back();
    reverse(ret.begin(), ret.end());
    cout << ret << endl;
    return 0;
}

```

