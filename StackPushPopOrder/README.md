# 题目: 栈的压入、弹出元素
## 题目描述：
输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如，序列 {1,2,3,4,5} 是某栈的压栈序列，序列 {4,5,3,2,1} 是该压栈序列对应的一个弹出序列，但 {4,3,5,1,2} 就不可能是该压栈序列的弹出序列。
# 本题考点：
  
  数据结构栈的编程能力
  
# 解题思路:
  此题与LeetCode-946题一样

   采用模拟法，借助一个辅助的栈，遍历压栈的顺序，依次放进辅助栈中。
   
   对于每一个放进栈中的元素，栈顶元素都与出栈的popIndex对应位置的元素进行比较，是否相等，相等则popIndex++，再判断，直到为空或者不相等为止。
   
   具体入栈出栈的操作模拟如下：
   ![1](https://github.com/bryceustc/CodingInterviews/blob/master/StackPushPopOrder/Images/1.png)
   
   ![2](https://github.com/bryceustc/CodingInterviews/blob/master/StackPushPopOrder/Images/2.png)
# 代码

[C++](./StackPushPopOrder.cpp)

[Python](./StackPushPopOrder.py)

# C++: 
### 
```c++
class Solution {
public:
    bool IsPopOrder(vector<int> pushV,vector<int> popV) {
        int n = pushV.size();
        int m = popV.size();
        if (n!=m) return false;
        // 辅助栈
        stack<int> s;
        //弹出序列的下表索引
        int index = 0;
        for (int i=0;i<n;i++)
        {
            //不停地将pushV中的元素压入栈中，一旦栈顶元素与popV相等了，则开始出栈
            //不相等则继续入栈
            s.push(pushV[i]);
            while(!s.empty() && s.top()==popV[index])
            {
                s.pop();
                index++;
            }
        }
        //栈中没有元素了说明元素全部一致，并且符合弹出顺序，那么返回true
        return s.empty();
    }
};
```

# Python:
### 
```python
# -*- coding:utf-8 -*-
class Solution:
    def IsPopOrder(self, pushV, popV):
        # write code here
        n = len(pushV)
        m = len(popV)
        if n!=m:
            return False
        s = []
        index = 0
        for i in range(n):
            s.append(pushV[i])
            while len(s)>0 and s[-1]==popV[index]:
                s.pop()
                index+=1
        if len(s)==0:
            return True
        return False
```
## 参考
  -  [LeetCode-946题-最小栈](https://github.com/bryceustc/LeetCode_Note/blob/master/cpp/Validate-Stack-Sequences/README.md)
  -  [Python栈实现](https://www.jianshu.com/p/1327cc0de255)


