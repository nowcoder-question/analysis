## 题目
[题目链接](https://www.nowcoder.com/practice/c451a3fd84b64cb19485dad758a55ebe?tpId=196&tqId=23251&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

##### 题目的主要信息：

- 找出所有和为S的连续正数序列，序列至少包括两个数
- 序列内按照从小至大的顺序，序列间按照开始数字从小到大的顺序

##### 举一反三：

学习完本题的思路你可以解决如下题目：

[JZ59. 滑动窗口的最大值](https://www.nowcoder.com/practice/1624bc35a45c42c0bc17d17fa0cba788?tpId=13&tqId=23458)

[JZ57. 和为S的两个数字](https://www.nowcoder.com/practice/390da4f7a00f44bea7c2f3d19491311b?tpId=13&tqId=23295)

##### 方法一：枚举（前置方法）
**思路：**

我们可以从数字1开始枚举连续的数字，将其累加判断其是否等于目标，如果小于目标数则继续往后累加，如果大于目标数说明会超过，跳出，继续枚举下一个数字开始的情况（比如2，比如3），这样每次都取连续的序列，只有刚好累加和等于目标数才可以记录从开始到结束这一串数字，代表是一个符合的序列。

```cpp
//枚举左区间
for(int i = 1; i <= up; i++){ 
    //从左区间往后依次连续累加
    for(int j = i; ;j++){
        ... 
```

而因为序列至少两个数，每次枚举区间的起始数字最多到目标数的一半向下取整即可，因为两个大于目标数一半的数相加一定大于目标数。

**具体做法：**

- step 1：从1到目标值一半向下取整作为枚举的左区间，即每次序列开始的位置。
- step 2：从每个区间首部开始，依次往后累加，如果大于目标值说明这一串序列找不到，换下一个起点。
- step 3：如果加到某个数字刚好等于目标值，则记录从区间首到区间尾的数字。

**Java实现代码：**
```java
import java.util.*;
public class Solution {
    public ArrayList<ArrayList<Integer> > FindContinuousSequence(int sum) {
        ArrayList<ArrayList<Integer> > res = new ArrayList<ArrayList<Integer> >();
        int sum1 = 0;
        //因为序列至少两个数，因此枚举最多到该数字的一半向下取整
        int up = (sum - 1) / 2; 
        //枚举左区间
        for(int i = 1; i <= up; i++){ 
            //从左区间往后依次连续累加
            for(int j = i; ;j++){ 
                sum1 += j;
                //大于目标和则跳出该左区间
                if(sum1 > sum){ 
                    sum1 = 0;
                    break;
                //等于则找到
                }else if(sum1 == sum){ 
                    sum1 = 0;
                    ArrayList<Integer> temp = new ArrayList<Integer>();
                    //记录线序的数字
                    for(int k = i; k <= j; k++) 
                        temp.add(k);
                    res.add(temp);
                    break;
                }
            }
        }
        return res;
    }
}
```
**C++实现代码：**
```cpp
class Solution {
public:
    vector<vector<int> > FindContinuousSequence(int sum) {
        vector<vector<int> > res;
        vector<int> temp;
        int sum1 = 0;
        //因为序列至少两个数，因此枚举最多到该数字的一半向下取整
        int up = (sum - 1) / 2; 
        //枚举左区间
        for(int i = 1; i <= up; i++){ 
            //从左区间往后依次连续累加
            for(int j = i; ;j++){ 
                sum1 += j;
                //大于目标和则跳出该左区间
                if(sum1 > sum){ 
                    sum1 = 0;
                    break;
                //等于则找到
                }else if(sum1 == sum){ 
                    sum1 = 0;
                    temp.clear();
                    //记录线序的数字
                    for(int k = i; k <= j; k++) 
                        temp.push_back(k);
                    res.push_back(temp);
                    break;
                }
            }
        }
        return res;
    }
};
```
**Python实现代码：**
```python
class Solution:
    def FindContinuousSequence(self , sum: int) -> List[List[int]]:
        res = []
        sum1 = 0
        #因为序列至少两个数，因此枚举最多到该数字的一半向下取整
        up = (sum - 1) // 2 
        #枚举左区间
        for i in range(1, up + 1):
            #从左区间往后依次连续累加
            j = i
            while True:
                sum1 += j
                #大于目标和则跳出该左区间
                if sum1 > sum: 
                    sum1 = 0
                    break
                #等于则找到
                elif sum1 == sum: 
                    sum1 = 0
                    temp = []
                    #记录线序的数字
                    for k in range(i, j + 1):
                        temp.append(k)
                    res.append(temp)
                    break
                j += 1
        return res
```
**复杂度分析：**
- 时间复杂度：$O(n\sqrt n)$，其中$n$为目标数字sum，外层枚举最多$\lfloor n/2 \rfloor$次，内层判断最多不会超过$O(\sqrt n)$，因为如果从1累加到$\sqrt n$会大于$n$，而从1累加到$\sqrt n -1$会小于$n$，因此最坏情况下累加$\sqrt n$次
- 空间复杂度：$O(\sqrt n)$，其中res属于返回答案必要空间，额外空间只有temp数组，最坏情况长度为$\sqrt n$


##### 方法二：滑动窗口（推荐使用）

**知识点：滑动窗口**

滑动窗口是指在数组、字符串、链表等线性结构上的一段，类似一个窗口，而这个窗口可以依次在上述线性结构上从头到尾滑动，且窗口的首尾可以收缩。我们在处理滑动窗口的时候，常用双指针来解决，左指针维护窗口左界，右指针维护窗口右界，二者同方向不同速率移动维持窗口。

**思路：**

从某一个数字开始的连续序列和等于目标数如果有，只能有一个，于是我们可以用这个性质来使区间滑动。

两个指针l、r指向区间首和区间尾，公式$(l + r) * (r - l + 1) / 2$计算区间内部的序列和，如果这个和刚好等于目标数，说明以该区间首开始的序列找到了，记录下区间内的序列，同时以左边开始的起点就没有序列了，于是左区间收缩；如果区间和大于目标数，说明该区间过长需要收缩，只能收缩左边；如果该区间和小于目标数，说明该区间过短需要扩大，只能向右扩大，移动区间尾。

**具体做法：**

- step 1：从区间$[1,2]$开始找连续子序列。
- step 2：每次用公式计算区间内的和，若是等于目标数，则记录下该区间的所有数字，为一种序列，同时左区间指针向右。
- step 3：若是区间内的序列和小于目标数，只能右区间扩展，若是区间内的序列和大于目标数，只能左区间收缩。

**图示：**

![alt](https://uploadfiles.nowcoder.com/images/20211203/397721558_1638516207843/20A9A4EB486B238F2B1D5DCE1BDF64B6)

**Java实现代码：**
```java
import java.util.*;
public class Solution {
    public ArrayList<ArrayList<Integer> > FindContinuousSequence(int sum) {
        ArrayList<ArrayList<Integer> > res = new ArrayList<ArrayList<Integer> >();
        //从1到2的区间开始
        for(int l = 1, r = 2; l < r;){ 
            //计算区间内的连续和
            int sum1 = (l + r) * (r - l + 1) / 2; 
            //如果区间内和等于目标数
            if(sum1 == sum){ 
                ArrayList<Integer> temp = new ArrayList<Integer>();
                //记录区间序列
                for(int i = l; i <= r; i++) 
                    temp.add(i);
                res.add(temp);
                //左区间向右
                l++; 
            //如果区间内的序列和小于目标数，右区间扩展
            }else if(sum1 < sum) 
                r++;
            //如果区间内的序列和大于目标数，左区间收缩
            else 
                l++;
        }
        return res;
    }
}
```
**C++实现代码：**
```cpp
class Solution {
public:
    vector<vector<int> > FindContinuousSequence(int sum) {
        vector<vector<int> > res;
        vector<int> temp;
        //从1到2的区间开始
        for(int l = 1, r = 2; l < r;){ 
            //计算区间内的连续和
            int sum1 = (l + r) * (r - l + 1) / 2; 
            //如果区间内和等于目标数
            if(sum1 == sum){ 
                temp.clear();
                //记录区间序列
                for(int i = l; i <= r; i++) 
                    temp.push_back(i);
                res.push_back(temp);
                //左区间向右
                l++; 
            //如果区间内的序列和小于目标数，右区间扩展
            }else if(sum1 < sum) 
                r++;
            //如果区间内的序列和大于目标数，左区间收缩
            else 
                l++;
        }
        return res;
    }
};
```
**Python实现代码：**
```python
class Solution:
    def FindContinuousSequence(self , sum: int) -> List[List[int]]:
        res = []
        #从1到2的区间开始
        l = 1
        r = 2
        while l < r:
            #计算区间内的连续和
            sum1 = (l + r) * (r - l + 1) / 2 
            #如果区间内和等于目标数
            if sum1 == sum:
                temp = []
                #记录区间序列
                for i in range(l, r + 1):
                    temp.append(i)
                res.append(temp)
                #左区间向右
                l += 1
            #如果区间内的序列和小于目标数，右区间扩展
            elif sum1 < sum: 
                r += 1
            #如果区间内的序列和大于目标数，左区间收缩
            else: 
                l += 1
        return res
```
**复杂度分析：**
- 时间复杂度：$O(n)$，区间移动次数最多$\lfloor n/2 \rfloor$次
- 空间复杂度：$O(\sqrt n)$，其中res属于返回答案必要空间，额外空间只有temp数组，最坏情况长度为$\sqrt n$



