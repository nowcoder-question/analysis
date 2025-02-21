## 题目
[题目链接](https://www.nowcoder.com/practice/4c776177d2c04c2494f2555c9fcc1e49?tpId=196&tqId=23268&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

##### 题目主要信息：

- 实现栈的push、pop、top、min函数
- **访问每个函数的时间复杂度为$O(1)$**

##### 举一反三：

学习完本题的思路你可以解决如下题目：

[BM42.用两个栈实现队列](https://www.nowcoder.com/practice/54275ddae22f475981afa2244dd448c6?tpId=295&sfm=html&channel=nowcoder)

##### 方法：双栈法（推荐使用）

**知识点：栈**

栈是一种仅支持在表尾进行插入和删除操作的线性表，这一端被称为栈顶，另一端被称为栈底。元素入栈指的是把新元素放到栈顶元素的上面，使之成为新的栈顶元素；元素出栈指的是从一个栈删除元素又称作出栈或退栈，它是把栈顶元素删除掉，使其相邻的元素成为新的栈顶元素。

**思路：**

我们都知道栈结构的push、pop、top操作都是$O(1)$，但是min函数做不到，于是想到在push的时候就将最小值记录下来，由于栈先进后出的特殊性，我们可以构造一个单调栈，保证栈内元素都是递增的，栈顶元素就是当前最小的元素。

**具体做法：**

- step 1：使用一个栈记录进入栈的元素，正常进行push、pop、top操作。
- step 2：使用另一个栈记录每次push进入的最小值。
- step 3：每次push元素的时候与第二个栈的栈顶元素比较，若是较小，则进入第二个栈，若是较大，则第二个栈的栈顶元素再次入栈，因为即便加了一个元素，它依然是最小值。于是，每次访问最小值即访问第二个栈的栈顶。
```java
//空或者新元素较小，则入栈
if(s2.isEmpty() || s2.peek() > node)  
    s2.push(node);
else
    //重复加入栈顶
    s2.push(s2.peek());
```

**图示：**

![图片说明](https://uploadfiles.nowcoder.com/images/20210717/397721558_1626504307685/BF24FE790ACB10342DE5628AEC3283ED "图片标题") 

**Java实现代码：**
```java
import java.util.Stack;

public class Solution {
    //用于栈的push 与 pop
    Stack<Integer> s1 = new Stack<Integer>(); 
    //用于存储最小min
    Stack<Integer> s2 = new Stack<Integer>(); 
    public void push(int node) {
        s1.push(node);  
        //空或者新元素较小，则入栈
        if(s2.isEmpty() || s2.peek() > node)  
            s2.push(node);
        else
            //重复加入栈顶
            s2.push(s2.peek());  
    }
    
    public void pop() {
        s1.pop();
        s2.pop();
    }
    
    public int top() {
        return s1.peek();
    }
    
    public int min() {
        return s2.peek();
    }
}
```
**C++实现代码：**
```cpp
class Solution {
public:
    //用于栈的push 与 pop
    stack<int> s1;  
    //用于存储最小min
    stack<int> s2;  
    void push(int value) {
        s1.push(value);  
        //空或者新元素较小，则入栈
        if(s2.empty() || s2.top() > value)  
            s2.push(value);
        else
            //重复加入栈顶
            s2.push(s2.top());  
    }
    void pop() {
        s1.pop();
        s2.pop();
    }
    int top() {
        return s1.top();
    }
    int min() {
        return s2.top();
    }
};
```
**Python代码实现：**
```Python
class Solution:
    def __init__(self):
        self.s1 = []
        self.s2 = []
    def push(self, node):
        self.s1.append(node)  
        #空或者新元素较小，则入栈
        if len(self.s2) == 0 or self.s2[-1] > node:  
            self.s2.append(node)
        else:
            #重复加入栈顶
            self.s2.append(self.s2[-1])  
    def pop(self):
        self.s1.pop()
        self.s2.pop()
    def top(self):
        return self.s1[-1]
    def min(self):
        return self.s2[-1]
```

**复杂度分析：**
- 时间复杂度：$O(1)$，每个函数访问都是直接访问，无循环
- 空间复杂度：$O(n)$，s1为必要空间，s2为辅助空间