## 用两个栈实现队列
### 信息卡片
- 时间：2020-04-03
- 题目链接：[用两个栈实现队列](https://www.nowcoder.com/practice/54275ddae22f475981afa2244dd448c6?tpId=13&tqId=11158&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
- tag：`queue` `stack`
### 题目描述
```
用两个栈来实现一个队列，完成队列的Push和Pop操作。 队列中的元素为int类型。
```
### 解法一　
#### 解题思路
这里搬运一下牛客网一位老兄的题解，写的很有条理。

队列的特性是：“先入先出”，栈的特性是：“先入后出”

当我们向模拟的队列插入数 a,b,c 时，假设插入的是 stack1，此时的栈情况为：

栈 stack1：{a,b,c}

栈 stack2：{}


当需要弹出一个数，根据队列的"先进先出"原则，a 先进入，则 a 应该先弹出。但是此时 a 在 stack1 的最下面，将 stack1 中全部元素逐个弹出压入 stack2，现在可以正确的从 stack2 中弹出 a，此时的栈情况为：

栈 stack1：{}

栈 stack2：{c,b}


继续弹出一个数，b 比 c 先进入"队列"，b 弹出，注意此时 b 在 stack2 的栈顶，可直接弹出，此时的栈情况为：

栈 stack1：{}

栈 stack2：{c}


此时向模拟队列插入一个数 d，还是插入 stack1，此时的栈情况为：

栈 stack1：{d}

栈 stack2：{c}


弹出一个数，c 比 d 先进入，c 弹出，注意此时 c 在 stack2 的栈顶，可直接弹出，此时的栈情况为：

栈 stack1：{d}

栈 stack2：{}


根据上述栗子可得出结论：

当插入时，直接插入 stack1。当弹出时，当 stack2 不为空，弹出 stack2 栈顶元素，如果 stack2 为空，将 stack1 中的全部数逐个出栈入栈 stack2，再弹出 stack2 栈顶元素.


```C++
class Solution
{
public:
    void push(int node) {
        stack1.push(node);
    }

    int pop() {
        int res;
        if(stack2.size()>0)
        {
            res=stack2.top();
            stack2.pop();
        }
        else if(stack1.size()>0)
        {
            int sta;
            while(stack1.size()>0)
            {
                sta=stack1.top();
                stack1.pop();
                stack2.push(sta);
            }
            res=stack2.top();
            stack2.pop();
        }
        return res;
    }

private:
    stack<int> stack1;
    stack<int> stack2;
};
```


### 知识扩展
```
用两个队列实现一个栈
```

#### 解题思路

队列的特性是：“先入先出”，栈的特性是：“先入后出”

当我们向模拟的栈插入数 a,b,c 时，假设插入的是 queue1，此时的队列情况为(a 位于队列头部，c 位于队列尾部)：

队列 queue1：{a,b,c}

队列 queue2：{}


当需要弹出一个数，根据栈的"先入后出"原则，最后被压入栈的 c 应该最先被弹出。由于 c 位于 queue1 的尾部，而我们每次只能从队列的头部删除元素，因此我们可以先从 queue1 中依次删除元素 a、b 并插入 queue2，再从 queue1 中删除元素 c。此时的栈情况为：

队列 queue1：{}

队列 queue2：{a,b}


继续弹出一个数，将 a 放入 queue1 中，从 queue2 中删除元素 b。此时的栈情况为：

队列 queue1：{a}

队列 queue2：{}


接下来往栈内压入一个元素 d。此时 queue1　已经有一个元素，把 d 插入 queue1的尾部。此时的栈情况为：

队列 queue1：{a,d}

队列 queue2：{}


再从栈内弹出一个元素，那么此时被弹出的应该是最后被压入的 d。由于 d 位于 queue1 的尾部，我们先将 a 插入到 queue2，再从 queue1 中删除 d。

队列 queue1：{}

队列 queue2：{a}



```C++
class MyStack {
public: /** Initialize your data structure here. */
    queue<int> q1,q2;
    MyStack() {   
    }
    /** Push element x onto stack. */
    void push(int x) {
        while(!q2.empty())
        {
            q1.push(q2.front());
            q2.pop();
        }
        q2.push(x);
        while(!q1.empty())
        {
            q2.push(q1.front());
            q1.pop();
        }
    }
    /** Removes the element on top of the stack and returns that element. */
    int pop() {
        int a=q2.front();
        q2.pop();
        return a;
    }
    /** Get the top element. */
    int top() {
        return q2.front();
    }  
    /** Returns whether the stack is empty. */
    bool empty() {
        return q2.empty();
    }
};
 
/**
 * Your MyStack object will be instantiated and called as such:
 * MyStack* obj = new MyStack();
 * obj->push(x);
 * int param_2 = obj->pop();
 * int param_3 = obj->top();
 * bool param_4 = obj->empty();
 */
```

