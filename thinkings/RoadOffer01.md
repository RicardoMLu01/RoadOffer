## 二维数组中的查找
### 信息卡片
- 时间：2020-03-16
- 题目链接：[二维数组中的查找](https://www.nowcoder.com/practice/abc3fe2ce8e146608e868a70efebf62e?tpId=13&tqId=11154&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
- tag：`search` `array`
### 题目描述
```
在一个二维数组中（每个一维数组的长度相同），每一行都按照从左到右递增的

顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样

的一个二维数组和一个整数，判断数组中是否含有该整数。
```
### 解法一　暴力法
#### 解题思路
这是最容易想到的，从左到右依次遍历整个二维数组，找到需要查找的数返回true，没找到就返回false。
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
**时间复杂度**：O(M*N)
**空间复杂度**：O(1)

### 解法二　左下／右上移动法
#### 解题思路
