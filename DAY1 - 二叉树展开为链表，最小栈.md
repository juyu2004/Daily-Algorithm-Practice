# DAY1 - 二叉树展开为链表，最小栈

### 二叉树展开为链表

给你二叉树的根结点 `root` ，请你将它展开为一个单链表：

- 展开后的单链表应该同样使用 `TreeNode` ，其中 `right` 子指针指向链表中下一个结点，而左子指针始终为 `null` 。
- 展开后的单链表应该与二叉树 [**先序遍历**](https://baike.baidu.com/item/先序遍历/6442839?fr=aladdin) 顺序相同。

这道题的难点就是先序的过程中，无法记录它的右节点

将二叉树展开为单链表之后，单链表中的结点顺序即为二叉树的前序遍历访问各节点的顺序。因此，可以对二叉树进行前序遍历，获得各节点被访问到的顺序。由于将二叉树展开为链表之后回破坏二叉树的结构，因此在前序遍历结束之后更新每个节点的左右节点的信息，将二叉树展开为单链表。

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
class Solution {
public:
    void flatten(TreeNode* root) {
        vector<TreeNode*> tmp;
        preordertraversal(root, tmp);
        for(int i = 1; i < tmp.size(); i++)
        {
            TreeNode* prev = tmp[i - 1], *cur = tmp[i];
            prev->left = nullptr;
            prev->right = cur;
        }
    }
    void preordertraversal(TreeNode* root, vector<TreeNode*> &tmp)
    {
        if(root != nullptr)
        {
            tmp.push_back(root);
            preordertraversal(root->left, tmp);
            preordertraversal(root->right, tmp);
        }
    }
};
```

这里补充一下二叉树前序遍历：

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
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> ret;
        preorder(root, ret);
        return ret;
    }

    void preorder(TreeNode* root, vector<int> &ret)
    {
        if(root == nullptr) return;
        ret.push_back(root->val);
        preorder(root->left, ret);
        preorder(root->right, ret);
    }
};
```

### 最小栈

这道题其实我做过，但是现在已经忘了~~

我们只需要设计一个数据结构，使得每个元素a与其相应的最小值m时刻保持一一对应。因此我们可以使用一个辅助栈，与元素栈同步插入与删除，用于存储每个元素对应的最小值。

* 当一个元素要入栈时，我们取当前辅助栈的栈顶存储的最小值，与当前元素比较得出的最小值，将这个最小值插入辅助栈中；
* 当一个元素要出栈时，我们把辅助栈的栈顶元素也一并弹出；
* 在任意一个时刻，栈内元素的最小值就存储在辅助栈的栈定元素中

```C++
class MinStack {
    stack<int> st;
    stack<int> tmp;
public:
    MinStack() {
        tmp.push(INT_MAX);
    }
    
    void push(int val) {
        st.push(val);
        tmp.push(min(val, tmp.top()));
    }
    
    void pop() {
        st.pop();
        tmp.pop();
    }
    
    int top() {
        return st.top();
    }
    
    int getMin() {
        return tmp.top();
    }
};

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack* obj = new MinStack();
 * obj->push(val);
 * obj->pop();
 * int param_3 = obj->top();
 * int param_4 = obj->getMin();
 */
```

