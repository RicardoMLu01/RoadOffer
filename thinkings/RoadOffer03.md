# 从尾到头打印链表
### 信息卡片
- 时间：2020-03-23
- 题目链接：[从尾到头打印链表](https://www.nowcoder.com/practice/d0267f7f55b3412ba93bd35cfa8e8035?tpId=13&tqId=11156&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
- tag：`list`
### 题目描述
```
输入一个链表，按链表从尾到头的顺序返回一个ArrayList。
```
### 解法一　调用reverse函数
#### 解题思路
这是一种简单粗暴的解法，先遍历一遍链表，在遍历链表的同时将当前指针指向的值放入vector数组中，直到链表遍历完成。最后，使用reverse函数将vector数组进行反转。
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


### 解法二　调用insert函数
#### 解题思路
与解法一有些类似，依次从前往后遍历链表，在遍历链表的同时，调用vector的insert函数从头部插入此时链表指针指向的值。最后就得到了一个反转的数组。
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

### 解法三　调用栈
#### 解题思路
创建一个元素为指针类型的栈，来存放链表的指针。在遍历链表的同时将指针入栈，此时栈的底部是链表的第一个元素，栈顶是链表的最后一个元素。弹出栈的时候把指针指向的值放入vector数组里面。
```C
class Solution {
public:
    vector<int> printListFromTailToHead(ListNode* head) {
        stack<ListNode*> stackNodes;
        vector<int> ArrayList;
        ListNode* p=head;
        while (p != NULL)
        {
            stackNodes.push(p);
            p = p->next;
        }
        while (!stackNodes.empty())
        {
            p=stackNodes.top();
            ArrayList.push_back(p->val);
            stackNodes.pop();
        }
        return ArrayList;
    }
};
```


### 解法三　递归
#### 解题思路
递归的本质就是一个栈结构，可以用递归来实现。要实现反过来输出的链表，我们每访问到一个节点的时候，先递归输出它后面的节点，再输出该节点自身，这样就得到反转的链表。
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

### 总结

解法四递归的方法看起来比较简洁，但是凡事都有利弊，当链表非常长的时候，就会导致函数调用的层级很深，从而有可能导致函数调用栈溢出。解法一、解法二、解法三要优于解法四。
