刷题：

​	101.判断二叉树是否是对称的

​	核心就是二棵子树，内侧和外侧之间判断是否相等

```C++
// 递归的方式
class Solution1 {
public:
    // 一种直接的思路是判断翻转后的二叉树和原来二叉树是否在元素上相等
    bool isSymmetric(TreeNode* root) {
        return compare(root->left, root->right);
    }

private:
    bool compare(TreeNode* left, TreeNode* right)
    {
        if(left==nullptr && right!=nullptr) return false;
        else if(left!=nullptr && right==nullptr) return false;
        else if(left==nullptr && right==nullptr) return true;
        else if(left->val != right->val) return false;
        bool outside = compare(left->left, right->right);
        bool inside = compare(left->right, right->left);
        bool reuslt = outside && inside;
        return reuslt;
    }
};

// 2 迭代的方式
class Solution {
public:
  
    bool isSymmetric(TreeNode* root) {
        if(root==nullptr){return true;}
        queue<TreeNode*> que;
        que.push(root->left);
        que.push(root->right); // 这里添加的顺序采用的是左子树，右子树节点方式
        while(!que.empty()){
            TreeNode* lhs = que.front();
            que.pop();
            TreeNode* rhs = que.front();
            que.pop();
            if(lhs == nullptr && rhs != nullptr){return false;}
            else if(lhs != nullptr && rhs == nullptr){return false;}
            else if(lhs == nullptr && rhs == nullptr){continue;}
            else if(lhs->val != rhs->val){return false;}
            que.push(lhs->left); // 内侧
            que.push(rhs->right); 
            que.push(lhs->right); // 外侧
            que.push(rhs->left);
        }
        return true;
    }
};
```

