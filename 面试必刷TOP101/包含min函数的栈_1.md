## 题目
[题目链接](https://www.nowcoder.com/practice/4c776177d2c04c2494f2555c9fcc1e49?tpId=295&tqId=23268&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

#描述
这是一篇针对初学者的题解。
知识点：栈
难度：一星
---

#题解
题目抽象：要求实现一个O(1)时间复杂度的返回最小值的栈。正常情况下，栈的push，pop操作都为O(1),
但是返回最小值，需要遍历整个栈，时间复杂度为O(n)，所以这里需要**空间换时间**的思想。

##方法：使用辅助栈
首先需要一个正常栈normal,用于栈的正常操作，然后需要一个辅助栈minval，专门用于获取最小值，具体操作如下。
![ ](https://uploadfiles.nowcoder.com/images/20200419/284295_1587290406796_0EDB8C9599BA026855B6DCCC1D5EDAE5 "图片标题") 
- push操作就如图片上操作。
- pop操作直接对应弹出就好了。
- top操作就返回normal的栈顶
- min操作就返回minval的栈顶

因此，代码如下：
```
class Solution {
public:
    stack<int> normal, minval;
    void push(int value) {
        normal.push(value);
        if (minval.empty()) {
            minval.push(value);
        }
        else {
            if (value &lt;= minval.top()) {
                minval.push(value);
            }
            else {
                minval.push(minval.top());
            }
        }
    }
    void pop() {
        normal.pop();
        minval.pop();
    }
    int top() {
        return normal.top();
    }
    int min() {
        return minval.top();
    }
};
```

时间复杂度：O(1)
空间复杂度：O(n), 开辟了一个辅助栈。</int>