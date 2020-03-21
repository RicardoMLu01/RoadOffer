# 替换空格
### 信息卡片
- 时间：2020-03-21
- 题目链接：[替换空格](https://www.nowcoder.com/practice/4060ac7e3e404ad1a894ef3e17650423?tpId=13&tqId=11155&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
- tag：`string`
### 题目描述
```
请实现一个函数，将一个字符串中的每个空格替换成“%20”。例如，当字符串
为We Are Happy.则经过替换之后的字符串为We%20Are%20Happy。
```
### 解法一　暴力法
#### 解题思路
将字符串从前往后遍历，每遇到一个空格需要将后面所有字符后移两个单位，直到遍历结束。
```C
class Solution {
public:
	void replaceSpace(char *str,int length) {
        if(str==NULL || length<=0)
            return;
        int k=0,len=0;
        int space_num=0;
        while(str[k]!='\0')
        {
            len++;
            if(str[k]==' ')
                space_num++;
            k++;
        }
        int len_add=len+2*space_num;
        if(len_add>=length)
            return;
        for(int i=0;i<len;i++)
        {
            if(str[i]==' ')
            {
                for(int j=len+1;j>=i+3;j--)
                    str[j]=str[j-2];
                str[i]='%';
                str[i+1]='2';
                str[i+2]='0';
                len+=2;
                i+=2;
            }
        }
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
	void replaceSpace(char *str,int length) {
        if(str==NULL || length<=0)
            return;
        int k=0,len=0;
        int space_num=0;
        while(str[k]!='\0')
        {
            len++;
            if(str[k]==' ')
                space_num++;
            k++;
        }
        int len_add=len+2*space_num;
        if(len_add>length)	//确保长度不超过最大容量
            return;
        int index_orignal=len;
        int index_new=len_add;
        while(index_orignal>=0 && index_new>index_orignal)
        {
            if(str[index_orignal]==' ')
            {
                str[index_new--]='0';
                str[index_new--]='2';
                str[index_new--]='%';
            }
            else
                str[index_new--]=str[index_orignal];
            --index_orignal;
        }
    }
};
```
**时间复杂度**：O(N)

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
