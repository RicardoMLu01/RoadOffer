## 重建二叉树
### 信息卡片
- 时间：2020-03-31
- 题目链接：[重建二叉树](https://www.nowcoder.com/practice/8a19cbe657394eeaac2f6ea9b0f6fcf6?tpId=13&tqId=11157&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
- tag：`binary tree`
### 题目描述
```
输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设输入的前序遍历

和中序遍历的结果中都不含重复的数字。例如输入前序遍历序列{1,2,4,7,3,5,6,8}

和中序遍历序列{4,7,2,1,5,3,8,6}，则重建二叉树并返回。
```
### 解法一　递归法
#### 解题思路

由于前序遍历的第一个数总是树的根节点的值，扫描中序遍历的序列，我们可以知道根节点的值在中序遍历序列的位置，进而我们知道树的左子树的中序遍历序列即为根节

点的值的左边序列，并且知道左子树的序列的长度， 树的右子树的中序遍历序列即为根节点的值的右边序列。由此左右子树的长度，我们可以从树的前序遍历序列中得到

左右子树的前序遍历。接下来以递归的方式再处理左右子树。


![image](https://github.com/RicardoMLu01/RoadOffer/blob/master/image_store/1.jpg)

![image](https://github.com/RicardoMLu01/RoadOffer/blob/master/image_store/2.jpg)


```C
/**
 * Definition for binary tree
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* reConstructBinaryTree(vector<int> pre,vector<int> vin) {
        if(pre.empty()||vin.empty())
            return NULL;
        TreeNode* head=new TreeNode(pre[0]);
        int root=0;
        for(int i=0;i<pre.size();i++)
        {
            if(pre[0]==vin[i])
            {
                root=i;
                break;
            }
        }
        vector<int> pre_left,pre_right,vin_left,vin_right;
        for(int i=0;i<root;i++)
        {
            pre_left.push_back(pre[i+1]);
            vin_left.push_back(vin[i]);
        }
        for(int i=root+1;i<pre.size();i++)
        {
            pre_right.push_back(pre[i]);
            vin_right.push_back(vin[i]);
        }
        head->left=reConstructBinaryTree(pre_left,vin_left);
        head->right=reConstructBinaryTree(pre_right,vin_right);
        return head;
    }
};
```

### 总结
1.一般树的问题都是使用递归的方式解决的。

2.熟悉二叉树的前序遍历和中序遍历。
