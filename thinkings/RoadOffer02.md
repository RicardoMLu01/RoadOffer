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
        int len_t=len+1;    //包含\0字符
        for(int i=0;i<len_t;i++)
        {
            if(str[i]==' ')
            {
                for(int j=len_t+1;j>=i+3;j--)
                    str[j]=str[j-2];
                str[i]='%';
                str[i+1]='2';
                str[i+2]='0';
                len_t+=2;
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


### 知识扩展

### 总结

在合并两个数组（包括字符串）时，如果从前往后复制每个数字（或字符）则需要重复移动数字（或字符）多次，那么我们可以考虑从后往前复制，这样就能减少移动的次数，从而提高效率。
