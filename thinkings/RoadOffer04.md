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
### 解法一　暴力法
#### 解题思路
由于前序遍历的第一个数总是树的根节点的值，扫描中序遍历的序列，我们可以知道根节点的值在中序遍历序列的位置，进而我们知道树的左子树的中序遍历序列即为根节点的值的左边序列，并且知道左子树的序列的长度， 树的右子树的中序遍历序列即为根节点的值的右边序列。由此左右子树的长度，我们可以从树的前序遍历序列中得到左右子树的前序遍历。接下来以递归的方式再处理左右子树。


!(image)[https://mp.weixin.qq.com/cgi-bin/filepage?type=2&begin=0&count=12&token=950094278&lang=zh_CN]


```C
class Solution {
public:
    bool Find(int target, vector<vector<int> > array) {
        int row,col;
        row=array.size();
        col=array[0].size();
        if(row==0||col==0)    //边界问题要考虑
            return false;
        for(int i=0;i<row;i++)
        {
            for(int j=0;j<col;j++)
            {
                if(array[i][j]==target)
                    return true;
            }
        }
        return false;
    }
};
```
**时间复杂度**：O(M*N)(统一说明，Ｍ代表行数，Ｎ代表列数，下同)

**空间复杂度**：O(1)

### 解法二　左下／右上移动法
#### 解题思路
利用二维数组的性质，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。

这里以右上移动为例，左下移动类似。二维数组右上角的数 m 是该行最大的数，是该列最小的数。

每次将 m 和目标值 target 比较：

当 m < target，由于 m 已经是该行最大的元素，想要更大只有从列考虑，取值下移一位

当 m > target，由于 m 已经是该列最小的元素，想要更小只有从行考虑，取值左移一位

当 m = target，找到该值，返回 true
```C
public:
    bool Find(int target, vector<vector<int> > array) {
        int row,col;
        row=array.size();
        col=array[0].size();
        if(row==0||col==0)
            return false;
        int i = 0;
        int j = col-1;
        while(i<row && j>=0){
            if(array[i][j] < target){
                i++;
            }else if(array[i][j] > target){
                j--;
            }else{
                return true;
            }
        }
        return false;
    }
};
```
**时间复杂度**：O(M+N)

**空间复杂度**：O(1)

### 解法三　二分查找
#### 解题思路
从第一行开始，对每一行进行二分查找。如果找到 target 则返回 true，没找到就移动到下一行进行二分查找，直到查找到最后一行。
```C
class Solution {
public:
    bool Find(int target, vector<vector<int> > array) {
        int row,col;
        row=array.size();
        col=array[0].size();
        if(row==0||col==0)
            return false;
        for(int i=0;i<row;i++)
        {
            int low=0;
            int high=col-1;
            while(low<=high)
            {
                int mid=low+(high-low)/2;
                if(target==array[i][mid])
                {
                    return true;
                }
                else if(target<array[i][mid])
                {
                    high=mid-1;
                }
                else
                {
                    low=mid+1;
                }
            }
        }
        return false;
    }
};
```
**时间复杂度**：O(MlogN)

**空间复杂度**：O(1)

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
