# 题目描述:数据流中的中位数
## 题目：
如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。我们使用Insert()方法读取数据流，使用GetMedian()方法获取当前读取数据的中位数。

# 本题考点：
  
  1). 插入排序/直接排序
  
  2). 大顶堆+小顶堆求解
  
  
# 解题思路:
  1). 直接排序 将数字存储在可调整大小的容器中。每次需要输出中间值时，对容器进行排序并输出中间值。平均时间复杂度为O(nlogn)+O(1)≈O(nlogn),空间复杂度：O(n) 线性空间，用于在容器中保存输入。
  
  2). 利用STL容器中的priority_queue 大顶堆+小顶堆进行求解 时间复杂度O(log n)+O(1)≈O(log n),空间复杂度，O(n) 用于在容器中保存输入的线性空间。

  3). 哪种算法允许将一个数字添加到已排序的数字列表中，但仍保持整个列表的排序状态？插入排序！

  我们假设当前列表已经排序。当一个新的数字出现时，我们必须将它添加到列表中，同时保持列表的排序性质。这可以通过使用二分搜索找到插入传入号码的正确位置来轻松实现。
（记住，列表总是排序的）。一旦找到位置，我们需要将所有较高的元素移动一个空间，以便为传入的数字腾出空间。

当插入查询的数量较少或者中间查找查询的数量大致相同。 此方法会很好地工作。 平均时间复杂度为O(n)+O(logn)≈O(n),空间复杂度：O(n) 线性空间，用于在容器中保存输入。

# 代码

[C++](./StreamMedian.cpp)

[Python](./StreamMedian.py)

# C++:
## 方法一：大顶堆+小顶堆
```c++
#include <iostream>
#include <vector>
#include <queue>
#include <algorithm>
using namespace std;
class Solution {
public:
	int count = 0;   // count要声明为成员变量
	void Insert(int num) {
		if (count % 2 == 0) // 偶数放入最大堆
		{
			max.push(num);
			int max_num = max.top();
			max.pop();
			min.push(max_num);
		}
		else     // 奇数放入最小堆
		{
			min.push(num);
			int min_num = min.top();
			min.pop();
			max.push(min_num);
		}
		count++;
	}

	double GetMedian() {
		if (count % 2 == 0)
		{
			return ((max.top() + min.top()) / 2.0);
		}
		else
		{
			return (min.top());
		}

	}
private:
	priority_queue<int, vector<int>> max;  //大顶堆
	priority_queue<int, vector<int>, greater<int>> min;  // 小顶堆
};

int main()
{
	Solution* obj = new Solution();
	obj->Insert(5);
	obj->Insert(2);
	obj->Insert(3);
	double res = obj->GetMedian();
	cout << res << endl;
	system("pause");
	return 0;
}
```


## 方法二：直接排序后返回

### （将数字存储在可调整大小的容器中。每次需要输出中间值时，对容器进行排序并输出中间值） 
### 时间复杂度:O(nlogn)+O(1)≃O(nlogn)
   1. 添加一个数字对于一个有效调整大小方案的容器来说需要花费 O(1)的时间。
   2. 找到中间值主要取决于发生的排序。对于标准比较排序，这需要 O(nlogn)时间
### 空间复杂度:O(n) 线性空间，用于在容器中保存输入。除了需要的空间之外没有其他空间（因为通常可以在适当的位置进行排序）
```c++
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

class Solution {
public:
	vector<double> store;
	void Insert(int num)
	{
		store.push_back(num);
	}

	double GetMedian()
	{
		sort(store.begin(), store.end());
		int n = store.size();
		if (n % 2 == 0)
		{
			return((store[n / 2] + store[n / 2 - 1]) / 2.0);
		}
		else
		{
			return (store[n / 2]);
		}
	}
};

int main()
{
	Solution* obj = new Solution();
	obj->Insert(5);
	obj->Insert(2);
	obj->Insert(3);
	double res = obj->GetMedian();
	cout << res << endl;
	system("pause");
	return 0;
}

```

## 方法三：插入排序
```c++
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

class Solution {
public:
	vector<double> store;
	void Insert(int num)
	{
    	    if (store.empty())
		  store.push_back(num);
    	    else
      		  store.insert(lower_bound(store.begin(),store.end(),num),num);
	}

	double GetMedian()
	{
		int n = store.size();
		if (n % 2 == 0)
		{
			return((store[n / 2] + store[n / 2 - 1]) / 2.0);
		}
		else
		{
			return (store[n / 2]);
		}
	}
};

int main()
{
	Solution* obj = new Solution();
	obj->Insert(5);
	obj->Insert(2);
	obj->Insert(3);
	double res = obj->GetMedian();
	cout << res << endl;
	system("pause");
	return 0;
}
```


# Python:
## 方法一：利用python 调用heapq库来实现大小堆进行求解
```python
# -*- coding:utf-8 -*-
import heapq
class Solution:
    def __init__(self):
        self.count = 0
        self.max_heap = []
        self.min_heap = []
    def Insert(self, num):
        # write code here
        if self.count % 2 == 0:
            heapq.heappush(self.max_heap,-num)
            max_heap_top = heapq.heappop(self.max_heap)
            heapq.heappush(self.min_heap,-max_heap_top)
        else:
            heapq.heappush(self.min_heap,num)
            min_heap_top = heapq.heappop(self.min_heap)
            heapq.heappush(self.max_heap,-min_heap_top)
        self.count+=1
    def GetMedian(self, data):
        # write code here
        if self.count % 2 == 0:
            return(self.min_heap[0]-self.max_heap[0])/2.0
        else:
            return self.min_heap[0]

if __name__ == '_ main__':
    nums = [4,5,1]
    
    Solution().Insert(num[0])
    res = Solution().GetMedian()
    print(res)
    Solution().Insert(num[1])
    res = Solution().GetMedian()    
    print(res)
    Solution().Insert(num[2])
    res = Solution().GetMedian()    
    print(res)
```

## 方法二：直接排序返回
```python
# -*- coding:utf-8 -*-
class Solution:
    def __init__(self):
        self.store=[]
    def Insert(self, num):
        # write code here
        self.store.append(num)
    def GetMedian(self, data):
        # write code here
        store=sorted(self.store)
        n = len(self.store)
        if n%2==0:
            return (self.store[n//2]+self.store[n//2-1])/2.0
        else:
            return self.store[n//2]

if __name__ == '_ main__':
    nums = [4,5,1]
    
    Solution().Insert(num[0])
    res = Solution().GetMedian()
    print(res)
    Solution().Insert(num[1])
    res = Solution().GetMedian()    
    print(res)
    Solution().Insert(num[2])
    res = Solution().GetMedian()    
    print(res)
```

## 方法三：插入排序
```python
# -*- coding:utf-8 -*-
class Solution:
    def __init__(self):
        self.store=[]
    def insertSort(self,arr):
        for i in range(1,len(arr)):
            j = i-1
            key = arr[i]
            while j >= 0:
                if arr[j] > key:
                    arr[j+1] = arr[j]
                    arr[j] = key
                j -= 1
        return arr
    def Insert(self, num):
        # write code here
        self.store.append(num)
	self.store = self.insertSort(self.store)
    def GetMedian(self, data):
        # write code here
        n = len(self.store)
        if n%2==0:
            return (self.store[n//2]+self.store[n//2-1])/2.0
        else:
            return self.store[n//2]

if __name__ == '_ main__':
    nums = [4,5,1]
    
    Solution().Insert(num[0])
    res = Solution().GetMedian()
    print(res)
    Solution().Insert(num[1])
    res = Solution().GetMedian()    
    print(res)
    Solution().Insert(num[2])
    res = Solution().GetMedian()    
    print(res)
```


# 参考：
   - [LeetCode-295数据流的中位数](https://github.com/bryceustc/LeetCode_Note/blob/master/cpp/Find-Median-From-Data-Stream/README.md)
   - [C/C++ | STL | 大顶堆 | 小顶堆 | std::priority_queue](https://blog.csdn.net/stone_fall/article/details/89010656) 
   - [C++ multiset通过greater、less指定排序方式，实现最大堆、最小堆功能](https://www.cnblogs.com/ficow/p/10045777.html) 
   - [c++ vector容器 插入元素时实现自动排序](https://blog.csdn.net/su20145104009/article/details/70955760)
   - [数据流的中位数](https://leetcode-cn.com/problems/find-median-from-data-stream/solution/shu-ju-liu-de-zhong-wei-shu-by-leetcode/)
   - [数据流中的中位数——牛客网](https://www.nowcoder.com/questionTerminal/9be0172896bd43948f8a32fb954e1be1?f=discussion)




