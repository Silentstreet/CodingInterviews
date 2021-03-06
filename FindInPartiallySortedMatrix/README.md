# 题目描述:二维数组中的查找

## 题目：
在一个二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，
判断数组中是否含有该整数

# 本题考点：
  
  1). 二维数组的理解
  
  2). 查找
  
# 解题思路:
  1). 直接暴力遍历二维数组所有元素，时间复杂度为O(m\*n)
  
  2). 对每一行使用一次二分查找，时间复杂度为O(m\*logn)
  
  3). 根据简单的例子寻找规律，从右上角开始寻找，时间复杂度为O(m+n)

# 代码

[C++](FindInPartiallySortedMatrix.cpp)

[Python](FindInPartiallySortedMatrix.py)

# C++:
## 方法一：暴力遍历
```c++


#include <iostream>
#include <vector>

class Solution{
    public:
        bool Find(vector<vector<int>> &nums,int target)
        {
            if (nums.empty()) return false;
            int m = nums.size();       
            int n = nums[0].size();
            for (int i=0;i<m;i++)
            {
                for (int j=0;j<n;j++)
                {
                    if (nums[i][j]==target)
                    {
                        return true;
                    }                    
                }
            }
            return false;          
        }
};


int main()
{
    vector<vector<int>> nums = {{1,2,3},{4,5,6},{7,8,9}};
    int target = 5;
    bool res;
    res = Solution().Find(nums,target);
    cout<< res <<endl;
    system("pause");
    return 0;
}
```

## 方法二：遍历行+二分查找：
```c++
#include <iostream>
#include <vector>
using namespace std;

class Solution{
    public:
        bool Find(vector<vector<int>> &nums,int target)
        {
            if (nums.empty()) return false;
            int n = nums.size();       
            int m = nums[0].size();
            for (int i=0;i<n;i++)
            {
                int start = 0;
                int end = m-1;
                while (end>=start)
                {
                    int mid = start + (end-start)/2;
                    if (nums[i][mid]==target)
                        return true;
                    if (nums[i][mid]<target)
                        start = mid+1;
                    if (nums[i][mid]>target)
                        end = mid-1;
                }
            }
            return false;          
        }
};


int main()
{
    vector<vector<int>> nums = {{1,2,3,10},{4,5,6,11},{7,8,9,13}};
    int target = 5;
    bool res;
    res = Solution().Find(nums,target);
    cout<< res <<endl;
    system("pause");
    return 0;
}
```

## 方法三：从右上角或者左下角开始查找：
```c++
#include <iostream>
#include <vector>
using namespace std;

// 右上角开始查找
class Solution{
    public:
        bool Find(vector<vector<int>> &nums,int target)
        {
            if (nums.empty()) return false;
            int m = nums.size();       
            int n = nums[0].size();
            int i=0,j=n-1;
            while (i<m && j>=0)
            {
                if (nums[i][j]==target)
                    return true;
                else if (nums[i][j]>target)
                    j--;//左移
                else
                    i++;//下移
            }
            
            return false;          
        }
};

// 左下角开始查找
class Solution{
    public:
        bool Find(vector<vector<int>> &nums,int target)
        {
            if (nums.empty()) return false;
            int m = nums.size();       
            int n = nums[0].size();
            int i=m-1,j=0;
            while (i>=0 && j<n)
            {
                if (nums[i][j]==target)
                    return true;
                if (nums[i][j]>target)
                    i--;//上移
                else
                    j++;//右移
            }
            
            return false;          
        }
};


int main()
{
    vector<vector<int>> nums = {{1,2,3,10},{4,5,6,11},{7,8,9,13}};
    int target = 5;
    bool res;
    res = Solution().Find(nums,target);
    cout<< res <<endl;
    system("pause");
    return 0;
}


```



# Python:
## 方法一：暴力遍历：
```python
# -*- coding:utf-8 -*-
class Solution:
    def Find(self, nums, target):
        m = len(nums)
        if m == 0:
            return False
        n = len(nums[0])
        for i in range(m):
            for j in range(n):
                if nums[i][j] == target:
                    return True
        return False


if __name__ == '_ main__':
    nums = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
    target = 5
    res = Solution().Find(nums, target)
    print(res)
```

## 方法二：一次二分查找：
```python
# -*- coding:utf-8 -*-
class Solution:
    def Find(self, nums, target):
        m = len(nums)
        if m == 0:
            return False
        n = len(nums[0])
        for i in range(m):
            start = 0
            end = n-1
            while end>=start:
                mid = start+(end-start)//2
                if nums[i][mid]==target:
                    return True
                if nums[i][mid]<target:
                    start=mid+1
                if nums[i][mid]>target:
                    end=mid-1
        return False


if __name__ == '_ main__':
    nums = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
    target = 5
    res = Solution().Find(nums, target)
    print(res)
```

## 方法三：sorted排序后返回：
```python
# -*- coding:utf-8 -*-

## 右上角查找
class Solution:
    def Find(self, nums, target):
        m = len(nums)
        if m == 0:
            return False
        n = len(nums[0])
        i=0
        j=n-1
        while i<m and j>=0 :
            if nums[i][j]==target:
                return True
            elif nums[i][j]>target:
                j-=1  ## 左移
            else:
                i+=1  ## 下移
        return False
## 左下角开始查找
class Solution:
    def Find(self, nums, target):
        m = len(nums)
        if m == 0:
            return False
        n = len(nums[0])
        i=m-1
        j=0
        while i>=0 and j<n :
            if nums[i][j]==target:
                return True
            elif nums[i][j]>target:
                i-=1 ##上移
            else:
                j+=1 ##右移
        return False
        
        
if __name__ == '_ main__':
    nums = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
    target = 5
    res = Solution().Find(nums, target)
    print(res)
```

# 参考：
 -  [二分查找算法](https://github.com/bryceustc/LeetCode_Note/blob/master/cpp/Find-First-And-Last-Position-Of-Element-In-Sorted-Array/BinarySearch.md)

