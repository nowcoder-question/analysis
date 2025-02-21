## 题目
[题目链接](https://www.nowcoder.com/practice/d11471c3bf2d40f38b66bb12785df47f?tpId=196&tqId=2283174&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

##### 题目的主要信息：

- 将一个字符串转换成一个整数，要求不能使用字符串转换整数的库函数
- 值为0或者字符串不是一个合法的数值则返回0
- 输入字符串包括数字字母符号,可以为空
- 第一个可能有正负号（可选），若是没有默认正数
- 要去掉前导空格和数字后续无用的字符
- 注意处理整数越界

##### 举一反三：

学习完本题的思路你可以解决如下题目：

[JZ20. 表示数值的字符串](https://www.nowcoder.com/practice/e69148f8528c4039ad89bb2546fd4ff8?tpId=13&tqId=1375424)

##### 方法一：遍历法（推荐使用）

**思路：**

既然是将字符串转化为数字，那我们可以遍历字符串，一个字符串，一个字符地检查，然后取出掉无用的，取出数字，利用如下代码，一个数字一个数字地转换，前面的扩大十倍加上后面一位。
```cpp
res = res * 10 + sign * (c - '0');
```

**具体做法：**

- step 1：遍历字符串，用index记录全程的下标。
- step 2：首先要排除空串，然后越过前导空格，以及前导空格后什么都没有就返回0.
- step 3：然后检查符号，没有符号默认为正数。
- step 4：再在后续遍历的时候，将数字字符转换成字符，遇到非数字则结束转换。
- step 5：与Int型最大最小值比较，检查越界情况。


**Java实现代码：**
```java
import java.util.*;
public class Solution {
    public int StrToInt (String s) {
        //空串
        if(s.isEmpty())
            return 0;
        int res = 0;
        int index = 0;
        int n = s.length();
        //去掉前导空格，如果有
        while(index < n){ 
            if(s.charAt(index) == ' ')
                index++;
            else
                break;
        }
        //去掉空格就什么都没有了
        if(index == n) 
            return 0;
        int sign = 1;
        //处理第一个符号是正负号的情况
        if(s.charAt(index) == '+')
            index++;
        else if(s.charAt(index) == '-'){
            index++;
            sign = -1;
        }
        //去掉符号就什么都没有了
        if(index == n) 
            return 0;
        while(index < n){
            char c = s.charAt(index);
            //后续非法字符，截断
            if(c < '0' || c > '9')  
                break;
            //处理越界
            if(res > Integer.MAX_VALUE / 10 || (res == Integer.MAX_VALUE / 10 && (c - '0') > Integer.MAX_VALUE % 10))
                return Integer.MAX_VALUE;
            if(res < Integer.MIN_VALUE / 10 || (res == Integer.MIN_VALUE / 10 && (c - '0') > -(Integer.MIN_VALUE % 10)))
                return Integer.MIN_VALUE;
            res = res * 10 + sign * (c - '0');
            index++;
        }
        return res;
    }
}
```
**C++实现代码：**
```cpp
class Solution {
public:
    int StrToInt(string s) {
        int res = 0;
        int index = 0;
        int n = s.length();
        //去掉前导空格，如果有
        while(index < n){ 
            if(s[index] == ' ')
                index++;
            else
                break;
        }
        //去掉空格就什么都没有了
        if(index == n) 
            return 0;
        int sign = 1;
        //处理第一个符号是正负号的情况
        if(s[index] == '+')
            index++;
        else if(s[index] == '-'){
            index++;
            sign = -1;
        }
        //去掉符号就什么都没有了
        if(index == n) 
            return 0;
        while(index < n){
            char c = s[index];
            //后续非法字符，截断
            if(c < '0' || c > '9')  
                break;
            //处理越界
            if(res > INT_MAX / 10 || (res == INT_MAX / 10 && (c - '0') > INT_MAX % 10))
                return INT_MAX;
            if(res < INT_MIN / 10 || (res == INT_MIN / 10 && (c - '0') > -(INT_MIN % 10)))
                return INT_MIN;
            res = res * 10 + sign * (c - '0');
            index++;
        }
        return res;
    }
};
```
**Python实现代码：**
```python
class Solution:
    def StrToInt(self , s: str) -> int:
        res = 0
        index = 0
        #去掉前导空格
        s = s.strip()
        #去掉空格就什么都没有了
        n = len(s)
        if s == "":
            return 0
        sign = 1
        #处理第一个符号是正负号的情况
        if s[index] == '+':
            index += 1
        elif s[index] == '-':
            index += 1
            sign = -1
        #去掉符号就什么都没有了
        if index == n:
            return 0
        while index < n:
            c = s[index]
            #后续非法字符，截断
            if c < '0' or c > '9':  
                break
            #转数字
            res = res * 10 + sign * ((int)(c) - (int)('0'))
            index += 1
        #输出处理越界
        return min(max(res, -2 ** 31), 2 ** 31 - 1)
```
**复杂度分析：**
- 时间复杂度：$O(n)$，其中$n$为字符串长度，一个遍历解决
- 空间复杂度：$O(1)$，常数级变量，无额外辅助空间使用


##### 方法二：状态机（扩展思路）
**思路：**

字符串无非就是这些类型：[ ' '（空格）, 0（前导或者数字中间的）, [1-9], 其它非法字符，'-/+' ]，我们可以将其映射成数字： [0,1,2,3,4]，一共有4种状态 0，1，2，3， 其中3退出状态机且返回当前保存的结果。

```java
//状态转移矩阵
int[][] states = {
{0,1,2,3,1},
{3,1,2,3,3},
{3,2,2,3,3}}; 
```

- step 1：利用常数矩阵保存状态机。
- step 2：遍历字符串，根据当前的字符类型，进入相应的状态。
- step 3：数字状态要进行转换，并判断是否超过int型上下界。

**图示：**

![alt](https://uploadfiles.nowcoder.com/images/20211003/397721558_1633266442882/7269932FDD7F8BA760B50D8A119A60C0)

**Java实现代码：**
```java
import java.util.*;
public class Solution {
    public int StrToInt (String s) {
        //状态转移矩阵
        int[][] states = {
            {0,1,2,3,1},
            {3,1,2,3,3},
            {3,2,2,3,3},
        }; 
        long res = 0;
        //与int边界比较
        long top = Integer.MAX_VALUE;  
        long bottom = Integer.MIN_VALUE;
        int n = s.length();
        int sign = 1;
        //状态从“ ”开始
        int state = 0; 
        for(int i = 0; i < n; i++){
            char c = s.charAt(i);
            if(c == ' ')
                //空格
                state = states[state][0]; 
            else if(c == '0')
                //前导0或者中间的0
                state = states[state][1]; 
            else if(c >= '1' && c <= '9') 
                //数字
                state = states[state][2]; 
            else if(c == '-' || c == '+'){ 
                //正负号
                state = states[state][4]; 
                if(state == 1)
                    sign = (c == '-') ? -1 : 1;
                else
                    break;
            }else 
                //非法字符
                state = states[state][3];  
            if(state == 2){
                //数字相加
                res = res * 10 + (c - '0'); 
                //越界处理
                res = (sign == 1) ? Math.min(res, top) : Math.min(res, -bottom); 
            }
            if(state == 3)
                break;
        }
        return (int)(sign * res);
    }
}
```
**C++实现代码：**
```cpp
class Solution {
public:
    int StrToInt(string s) {
        //状态转移矩阵
        vector<vector<int> > states = {
            {0,1,2,3,1},
            {3,1,2,3,3},
            {3,2,2,3,3},
        }; 
        long res = 0;
        //与int边界比较
        long top = INT_MAX;  
        long bottom = INT_MIN;
        int n = s.length();
        int sign = 1;
        //状态从“ ”开始
        int state = 0; 
        for(int i = 0; i < n; i++){
            char c = s[i];
            if(c == ' ')
                //空格
                state = states[state][0]; 
            else if(c == '0')
                //前导0或者中间的0
                state = states[state][1]; 
            else if(c >= '1' && c <= '9') 
                //数字
                state = states[state][2]; 
            else if(c == '-' || c == '+'){ 
                //正负号
                state = states[state][4]; 
                if(state == 1)
                    sign = (c == '-') ? -1 : 1;
                else
                    break;
            }else 
                //非法字符
                state = states[state][3];  
            if(state == 2){
                //数字相加
                res = res * 10 + int(c - '0'); 
                //越界处理
                res = (sign == 1) ? min(res, top) : min(res, -bottom); 
            }
            if(state == 3)
                break;
        }
        return (int)sign * res;
    }
};
```
**Python实现代码：**
```python
class Solution:
    def StrToInt(self , s: str) -> int:
        #状态转移矩阵
        states = [[0,1,2,3,1],
                 [3,1,2,3,3],
                 [3,2,2,3,3]]
        res = 0
        #与int边界比较
        top = 2 ** 31 - 1  
        bottom = -2 ** 31
        n = len(s)
        sign = 1
        #状态从“ ”开始
        state = 0 
        for i in range(n):
            c = s[i]
            if c == ' ':
                #空格
                state = states[state][0] 
            elif c == '0':
                #前导0或者中间的0
                state = states[state][1] 
            elif c >= '1' and c <= '9': 
                #数字
                state = states[state][2] 
            elif c == '-' or c == '+': 
                #正负号
                state = states[state][4] 
                if state == 1:
                    sign = -1 if(c == '-') else 1
                else:
                    break
            else:
                #非法字符
                state = states[state][3]  
            if state == 2:
                #数字相加
                res = res * 10 + ((int)(c) - (int)('0')) 
                #越界处理
                res = min(res, top) if(sign == 1) else min(res, -bottom) 
            if state == 3:
                break
        return sign * res
```
**复杂度分析：**
- 时间复杂度：$O(n)$，其中$n$为字符串长度，遍历一次字符串
- 空间复杂度：$O(1)$，状态矩阵属于常数空间