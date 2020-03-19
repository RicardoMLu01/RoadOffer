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
这是最容易想到的，从左到右依次遍历整个二维数组，找到需要查找的数返回 true，没找到就返回 false。
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
2.题目已经给了一个特定顺序的数组，如何自己使用 vector 来实现？
### 总结
1.在数据量很大的情况下，三种常见解法的时间复杂度相比，解法二 < 解法三 < 解法一，换句话说，解法二最好，解法三次之，解法三最差。
2.在做题的时候优先考虑边界问题，这样能更好地把问题考虑清楚。
