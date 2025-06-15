# 大数加法

```C++
class Solution {
public:
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 计算两个数之和
     * @param s string字符串 表示第一个整数
     * @param t string字符串 表示第二个整数
     * @return string字符串
     */
    string solve(string s, string t) {
        int len1 = s.size();
        int len2 = t.size();

        while(len1 < len2)
        {
            s = '0' + s;
            len1++;
        }

        while(len1 > len2)
        {
            t = '0' + t;
            len2++;
        }

        int carry = 0;
        string ans;

        // 从后面往前加
        for(int i = len1 - 1; i >= 0; i--)
        {
            int tmp = (s[i] - '0' + t[i] - '0' + carry);
            ans += char(tmp % 10 + '0');
            carry = tmp / 10;
        }
        // 注意末尾进位
        if(carry) ans += (carry + '0');
        // 最后得到的是逆序，需要反转
        reverse(ans.begin(), ans.end());

        return ans;
    }
};
```

大数相加，细节还是挺多的~