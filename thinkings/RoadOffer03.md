# 从尾到头打印链表
### 信息卡片
- 时间：2020-03-23
- 题目链接：[从尾到头打印链表](https://www.nowcoder.com/practice/d0267f7f55b3412ba93bd35cfa8e8035?tpId=13&tqId=11156&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
- tag：`list`
### 题目描述
```
输入一个链表，按链表从尾到头的顺序返回一个ArrayList。
```
### 解法一　暴力法
#### 解题思路
简单粗暴，先遍历一遍链表，在遍历链表的同时将当前指针指向的值放入vector数组中，直到链表遍历完成。最后，使用reverse函数将vector数组进行反转。
```C
public:
    vector<int> printListFromTailToHead(ListNode* head) {
        vector<int>ArrayList;
        if(!head || !head->next)
            return {};
        while(head)
        {
            ArrayList.push_back(head->val);
            head=head->next;
        }
        reverse(ArrayList.begin(),ArrayList.end());
        return ArrayList;
    }
};
```
**时间复杂度**：O(N^2)

**空间复杂度**：O(1)

### 解法二　双指针从后往前遍历
#### 解题思路
我们可以先遍历一次字符串，这样就能统计出字符串中空格的总数，并可以由此计算出替换之后的字符串的总长度。每替换一个空格，长度增加２，因此替换以后字符串的长度等于原来的长度加上２乘以空格数目。我们从字符串的后面开始复制和替换。首先准备两个指针：P1和P2。P1指向原始字符串的末尾，而P2指向替换之后的字符串的末尾。向前移动指针P1，逐个把它指向的字符复制到P2指向的位置，直到碰到第一个空格为止。碰到第一个空格之后，把P1向前移动１格，在P2之前插入“%20”，由于“%20”的长度为３，同时也要把P2向前移动３格。后面以此类推，直到P1和P2指向同一个位置，表明所有空格都已经替换完毕。
```C
class Solution {
public:
    vector<int> printListFromTailToHead(ListNode* head) {
        vector<int>ArrayList;
        if(head)
        {
            ArrayList.insert(ArrayList.begin(),head->val);
            while(head->next)
            {
                ArrayList.insert(ArrayList.begin(),head->next->val);
                head=head->next;
            }
        }
        return ArrayList;
    }
};
```
**时间复杂度**：O(N)

**空间复杂度**：O(1)

```C
class Solution {
public:
    vector<int> printListFromTailToHead(ListNode* head) {
        stack<ListNode*> stackNodes;
        vector<int> res;
        ListNode* p=head;
        // 单链表元素依次入栈
        while (p != NULL)
        {
            stackNodes.push(p);
            p = p->next;
        }
        // 栈中的单链表元素依次出栈
        while (!stackNodes.empty())
        {
            p=stackNodes.top();
            res.push_back(p->val);
            stackNodes.pop();
        }
         
         
        return res;
    }
};
```
```C
class Solution {
public:
     
    vector<int> printListFromTailToHead(struct ListNode* head) {
        vector<int> vec;
        printListFromTailToHead(head,vec);
        return vec;
    }
    void printListFromTailToHead(struct ListNode* head,vector<int> &vec)
    {
        if(head!=nullptr)
        {
            if(head->next!=nullptr)
            {
                printListFromTailToHead(head->next,vec);
            }
            vec.push_back(head->val);
        }
    }
};
```
### 知识扩展

### 总结

在合并两个数组（包括字符串）时，如果从前往后复制每个数字（或字符）则需要重复移动数字（或字符）多次，那么我们可以考虑从后往前复制，这样就能减少移动的次数，从而提高效率。
