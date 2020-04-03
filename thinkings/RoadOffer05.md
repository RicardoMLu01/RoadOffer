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

当插入时，直接插入 stack1
当弹出时，当 stack2 不为空，弹出 stack2 栈顶元素，如果 stack2 为空，将 stack1 中的全部数逐个出栈入栈 stack2，再弹出 stack2 栈顶元素

```C
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
1.题解中使用的是 C++ 的容器 vector 来操作数组，这种容器与平常使用的内置数组相比有什么优势呢？各自的应用场合是哪里？

优势：vector 支持动态扩容，适用于数组长度不断改变的情况；内置数组什么都需要亲力亲为，经常会出现数组越界的情况，而 vector 可以使用迭代器避免；vector 支持直接拷贝和赋值，内置数组无法进行直接拷贝和赋值。

劣势：vector 存储大量数据时效率比内置数组低，且在分配连续大量内存时可能会失败。

2.题目已经给了一个特定顺序的数组，如何自己使用 vector 进行行或列排序？
```C
#include<stdio.h>
#include<algorithm>
#include<vector>
#include<stdlib.h>
#include<iostream>
using namespace std;
 
int main()
{
	vector<vector<int>> viA(10);
	for (int i = 0; i < 10;i++)
		for (int j = 0; j < 10; j++){
			viA[i].push_back(rand()%100);
		}
	for (int i = 0; i < 10; i++){
		for (int j = 0; j < 10; j++){
			cout << viA[i][j] << "\t";
		}
		cout << endl;
	}
	cout << "按行排序后的输出" << endl;
	for (int i = 0; i < 10; i++){
		sort(viA[i].begin(), viA[i].end());//默认为从小到大排序
	}
	for (int i = 0; i < 10; i++){
		for (int j = 0; j < 10; j++){
			cout << viA[i][j] << "\t";
		}
		cout << endl;
	}
	while (1);
		
	return 0;
}
```
### 总结
1.在数据量很大的情况下，三种常见解法的时间复杂度相比，解法二 < 解法三 < 解法一，换句话说，解法二最好，解法三次之，解法三最差。

2.在做题的时候优先考虑边界问题，这样能更好地把问题考虑清楚。
