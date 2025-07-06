# DAY - 10

## 游游的you

```C++
#include <iostream>
using namespace std;

int main() 
{
    int n;
    cin >> n;
    while(n--)
    {
        int a, b, c;
        cin >> a >> b >> c;
        int sum = 0;
        while(a >= 1 && b >= 1 && c >= 1) 
        {
            sum += 2;
            a--;
            b--;
            c--;
        }
        if(b >= 2) sum += (b - 1);
        cout << sum << endl;
    }
    return 0;
}
```

由题意得:

* you 和 oo 是相互独立的
* 但是 you 的分值更高, 因此我们应该优先去拼凑 you, 然后再考虑 oo

## 腐烂的苹果

```C++
class Solution 
{
public:
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param grid int整型vector<vector<>> 
     * @return int整型
     */
    int m, n;
    int dx[4] = {0, 0, 1, -1};
    int dy[4] = {1, -1, 0, 0};
    bool vis[1010][1010] = {0};

    int rotApple(vector<vector<int> >& grid) 
    {
        m = grid.size(), n = grid[0].size();
        queue<pair<int, int>> q;

        for(int i = 0; i < m; ++i)
            for(int j = 0; j < n; ++j)
                if(grid[i][j] == 2) q.push({i, j});

        int ret = 0;
        while(!q.empty())
        {
            int sz = q.size();
            ret++;
            while(sz--)
            {
                auto [a, b] = q.front();
                q.pop();
                // 注意这是循环, 
                for(int k = 0; k < 4; k++) 
                {
                    int x = a + dx[k];
                    int y = b + dy[k];
                    if(x >= 0 && x < m && y >= 0 && y < n && !vis[x][y] && grid[x][y] == 1) 
                    {
                        vis[x][y] = true;
                        q.push({x, y});
                    }
                }
            }
        }

        for(int i = 0; i < m; i++)
            for(int j = 0; j < n; j++)
                if(grid[i][j] == 1 && !vis[i][j]) return -1;

        return ret - 1;
    }
};
```

## **JZ62** **孩子们的游戏(圆圈中最后剩下的数)**

https://www.nowcoder.com/practice/f78a359491e64a50bce2d89cff857eb6?tpId=13&tqId=11199&ru=/exam/oj

```C++
class Solution 
{
public:
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param n int整型 
     * @param m int整型 
     * @return int整型
     */
    int LastRemaining_Solution(int n, int m) 
    {
        vector<int> nums(n); // 用 vector 动态管理, 比固定数组更灵活
        for(int i = 0; i < n; i++)
            nums[i] = i;

        int idx = 0; // 记录当前起始位置
        while(nums.size() > 1)
        {
            idx = (idx + m - 1) % nums.size(); // 计算要删除的索引
            nums.erase(nums.begin() + idx); // 删除元素, 后续元素前移
        }

        return nums[0];
    }
};
```

用 vector 动态管理

**erase 删除元素, 后续元素前移**

```C++
class Solution 
{
public:
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param n int整型 
     * @param m int整型 
     * @return int整型
     */
    int LastRemaining_Solution(int n, int m) 
    {
        list<int> nums;
        for(int i = 0; i < n; ++i) nums.push_back(i);
        
        auto it = nums.begin(); // 迭代器模拟指针, 指向当前起始位置
        while(nums.size() > 1)
        {
            for(int i = 0; i < m - 1; i++)
            {
                it++;
                if(it == nums.end()) it = nums.begin(); // 循环链表, 到末尾回到开头
            }
            // 删除当前节点, it 自动指向下一个节点的位置 (或重新定位, 因 list 删除后迭代器失效需处理, 不过 C++11 后可简化)
            it = nums.erase(it);
            if(it == nums.end()) it = nums.begin(); // 若删到末尾, 重置到开头
        }

        return *nums.begin();
    }
};
```

用 list 解决问题, erase 删除最后一个节点指向为 nullptr , 也就是 nums.end()

```C++
class Solution 
{
public:
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param n int整型 
     * @param m int整型 
     * @return int整型
     */
    int LastRemaining_Solution(int n, int m) 
    {
        int f = 0;  // 当 n=1 时，结果为 0
        for(int i = 2; i <= n; i++)  // 从 n=2 开始递推到 n
            f = (f + m) % i;  // 递推公式：f(n) = (f(n-1) + m) % n
        return f;  // 返回最终结果
    }
};
```

推导过程:

当 n = 1 时:

圈中只有一个人, 编号为 0, 因此 f(1, m) = 0

当 n >= 2 时:

第一轮报数后, 编号为 (m - 1) % n 的人出列, 剩下 n - 1 个人.

从出列的下一个人 (编号为 m % n) 开始, 重新编号为 0, 1, 2, ..., n - 2

此时问题转化为 n -1 个人的子问题, 子问题的解为 f(n - 1, m)

将子问题的解映射回原问题的编号, 得到递归公式:

f(n, m) = (f(n - 1, m) + m) % n

## 随机链表的复制

```C++
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* next;
    Node* random;
    
    Node(int _val) {
        val = _val;
        next = NULL;
        random = NULL;
    }
};
*/

class Solution 
{
public:
    Node* copyRandomList(Node* head) 
    {
        if(!head) return nullptr;

        // 哈希表: 原节点 -> 新节点
        unordered_map<Node*, Node*> map;

        // 第一步: 复制节点值并建立映射
        Node* cur = head;
        while(cur)
        {
            map[cur] = new Node(cur->val);
            cur = cur->next;
        }

        // 第二步: 复制 next 和 random 指针
        cur = head;
        while(cur)
        {
            if(cur->next) map[cur]->next = map[cur->next];
            if(cur->random) map[cur]->random = map[cur->random];
            cur = cur->next;
        }

        return map[head];
    }
};
```

## LRU 缓存

https://leetcode.cn/problems/lru-cache/?envType=study-plan-v2&envId=top-100-liked

### 实现思路: 哈希表 + 双向链表

数据结构选择:

* 双向链表:

特性: 按 "最近使用" 排序, 头部是最近使用元素, 尾部是最久未使用元素.

作用: 支持 O(1) 时间插入, 删除(需记录前驱和后继).

* 哈希表 (unordered_map) : 

特性: 键值对存储, 支持 O(1) 时间查找.

作用: 快速根据 key 定位到链表中的节点.

### 核心逻辑

get 操作: 

1. 哈希表查找 key , 若不存在返回 -1
2. 若存在, 该节点移动到链表头部 (标记为最近使用)

put 操作:

1. 哈希表查找 key, 若存在则更新值, 并移动到链表头部.
2. 若不存在, 创建新节点:
   1. 插入链表头部 (标记为最近使用)
   2. 若容量超限, 删除链表尾部节点 (最久未使用), 并从哈希表中移除对应 key

链表节点结构: 需存储 key, value, 以及前驱(prev), 后继(next) 指针.

```C++
#include <iostream>
using namespace std;

// 双向链表节点结构
struct Node
{
    int key, value;
    Node* prev;
    Node* next;
    Node(int k, int v) : key(k), value(v), prev(nullptr), next(nullptr) {}
};

class LRUCache 
{
private:
    int capacity; // 缓存最大容量
    unordered_map<int, Node*> cache; // 哈希表: key->节点指针
    Node* head; // 链表头 (最近使用)
    Node* tail; // 链表尾 (最久未使用)

    // 辅助数组: 将节点移动到链表头部
    void moveToHead(Node* node)
    {
        if(node == head) return; // 已是头部, 无需操作
        // 断开原节点的前驱和后继
        if(node == tail)
        {
            tail = node->prev;
            tail->next = nullptr;
        }
        else
        {
            node->prev->next = node->next;
            node->next->prev = node->prev;
        }
        // 插入到头部
        node->next = head;
        head->prev = node;
        head = node;
        node->prev = nullptr;
    }

    // 辅助函数: 删除尾部节点 (最久未使用)
    void removeTail()
    {
        Node* oldTail = tail;
        tail = oldTail->prev;
        if(tail) tail->next = nullptr;
        else head = nullptr; // 缓存空了
        cache.erase(oldTail->key); // 从哈希表移除
        delete oldTail; // 释放内存
    }
public:
    LRUCache(int capacity) : capacity(capacity)
    {
        head = nullptr;
        tail = nullptr;
    }
    
    int get(int key) 
    {
        if(cache.find(key) == cache.end()) return -1; // 未找到
        Node* node = cache[key];
        moveToHead(node); // 标记未最近使用
        return node->value;
    }
    
    void put(int key, int value) 
    {
        if(cache.find(key) != cache.end())
        {
            // 已存在, 更新值并移动到头部
            Node* node = cache[key];
            node->value = value;
            moveToHead(node);
            return;
        }
        // 不存在, 创建新节点
        Node* newnode = new Node(key, value);
        cache[key] = newnode;
        // 插入到头部
        if(!head) 
        {
            head = newnode;
            tail = newnode;
        }
        else
        {
            newnode->next = head;
            head->prev = newnode;
            head = newnode;
        }
        // 检查容量, 超限制删除尾部
        if(cache.size() > capacity) removeTail();
    }
};

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache* obj = new LRUCache(capacity);
 * int param_1 = obj->get(key);
 * obj->put(key,value);
 */
```

