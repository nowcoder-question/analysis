## 题目
[题目链接](https://www.nowcoder.com/practice/ce73540d47374dbe85b3125f57727e1e?tpId=196&tqId=653&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

##### 题目主要信息：

- 有一个只包含数字的字符串，将该字符串转化成IP地址的形式
- 需要返回所有情况，顺序没有问题

##### 举一反三：

本题属于递归+回溯剪枝的类型，动态规划也可以完成，但是不如递归回溯剪枝的解释性强，因此为其他可用递归回溯方式处理的题目作参考

##### 方法一：枚举（推荐使用）

**思路：**

对于IP字符串，如果只有数字，则相当于需要我们将IP地址的三个点插入字符串中，而第一个点的位置只能在第一个字符、第二个字符、第三个字符之后，而第二个点只能在第一个点后1-3个位置之内，第三个点只能在第二个点后1-3个位置之内，且要要求第三个点后的数字数量不能超过3，因为IP地址每位最多3位数字。

**具体做法：**

- step 1：依次枚举这三个点的位置。
- step 2：然后截取出四段数字。
- step 3：比较截取出来的数字，不能大于255，且除了0以外不能有前导0，然后才能组装成IP地址加入答案中。

**Java实现代码：**
```java
import java.util.*;
public class Solution {
    public ArrayList<String> restoreIpAddresses (String s) {
        ArrayList<String> res = new ArrayList<String>();
        int n = s.length();
        //遍历IP的点可能的位置（第一个点）
        for(int i = 1; i < 4 && i < n - 2; i++){ 
            //第二个点的位置
            for(int j = i + 1; j < i + 4 && j < n - 1; j++){ 
                //第三个点的位置
                for(int k = j + 1; k < j + 4 && k < n; k++){ 
                    //最后一段剩余数字不能超过3
                    if(n - k >= 4) 
                        continue; 
                    //从点的位置分段截取
                    String a = s.substring(0, i);
                    String b = s.substring(i, j);
                    String c = s.substring(j, k);
                    String d = s.substring(k);
                    //IP每个数字不大于255
                    if(Integer.parseInt(a) > 255 || Integer.parseInt(b) > 255 || Integer.parseInt(c) > 255 || Integer.parseInt(d) > 255)
                        continue;
                    //排除前导0的情况
                    if((a.length() != 1 && a.charAt(0) == '0') || (b.length() != 1 && b.charAt(0) == '0') ||  (c.length() != 1 && c.charAt(0) == '0') || (d.length() != 1 && d.charAt(0) == '0'))
                        continue;
                    //组装IP地址
                    String temp = a + "." + b + "." + c + "." + d; 
                    res.add(temp);
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
    vector<string> restoreIpAddresses(string s) {
        vector<string> res;
        int n = s.length();
        //遍历IP的点可能的位置（第一个点）
        for(int i = 1; i < 4 && i < n - 2; i++){ 
            //第二个点的位置
            for(int j = i + 1; j < i + 4 && j < n - 1; j++){ 
                //第三个点的位置
                for(int k = j + 1; k < j + 4 && k < n; k++){
                    //最后一段剩余数字不能超过3
                    if(n - k >= 4) 
                        continue; 
                    //从点的位置分段截取
                    string a = s.substr(0, i);
                    string b = s.substr(i, j - i);
                    string c = s.substr(j, k - j);
                    string d = s.substr(k);
                    //IP每个数字不大于255
                    if(stoi(a) > 255 || stoi(b) > 255 || stoi(c) > 255 || stoi(d) > 255) 
                        continue;
                    //排除前导0的情况
                    if((a.length() != 1 && a[0] == '0') || (b.length() != 1 && b[0] == '0') ||  (c.length() != 1 && c[0] == '0') || (d.length() != 1 && d[0] == '0'))
                        continue;
                    //组装IP地址
                    string temp = a + "." + b + "." + c + "." + d; 
                    res.push_back(temp);
                }
            }
        }
        return res;
    }
};
```
**Python代码实现：**
```Python
class Solution:
    def restoreIpAddresses(self , s: str) -> List[str]:
        res = []
        n = len(s)
        i = 1
        #遍历IP的点可能的位置（第一个点）
        while i < 4 and i < n - 2: 
            j = i + 1
            #第二个点的位置
            while j < i + 4 and j < n - 1: 
                k = j + 1
                #第三个点的位置
                while k < j + 4 and k < n: 
                    #最后一段剩余数字不能超过3
                    if n - k >= 4: 
                        k += 1
                        continue 
                    #从点的位置分段截取
                    a = s[0: i]
                    b = s[i: j]
                    c = s[j: k]
                    d = s[k:]
                    #IP每个数字不大于255
                    if int(a) > 255 or int(b) > 255 or int(c) > 255 or int(d) > 255: 
                        k += 1
                        continue
                    #排除前导0的情况
                    if (len(a) != 1 and a[0] == '0') or (len(b) != 1 and b[0] == '0') or  (len(c) != 1 and c[0] == '0') or (len(d) != 1 and d[0] == '0'):
                        k += 1
                        continue
                    #组装IP地址
                    temp = a + "." + b + "." + c + "." + d 
                    res.append(temp)
                    k += 1
                j += 1
            i += 1
        return res
```

**复杂度分析：**
- 时间复杂度：如果将3看成常数，则复杂度为$O(1)$，如果将3看成字符串长度的1/4，则复杂度为$O(n^3)$，三次嵌套循环
- 空间复杂度：如果将3看成常数，则复杂度为$O(1)$，如果将3看成字符串长度的1/4，则复杂度为$O(n)$，4个记录截取字符串的临时变量。res属于返回必要空间。


##### 方法二：递归+回溯（扩展思路）

**知识点：递归**

递归是一个过程或函数在其定义或说明中有直接或间接调用自身的一种方法，它通常把一个大型复杂的问题层层转化为一个与原问题相似的规模较小的问题来求解。因此递归过程，最重要的就是查看能不能讲原本的问题分解为更小的子问题，这是使用递归的关键。

**思路：**

对于IP地址每次取出一个数字和一个点后，对于剩余的部分可以看成是一个子问题，因此可以使用递归和回溯将点插入数字中。

**具体做法：**

- step 1：使用step记录分割出的数字个数，index记录递归的下标，结束递归是指step已经为4，且下标到达字符串末尾。
- step 2：在主体递归中，每次加入一个字符当数字，最多可以加入三个数字，剩余字符串进入递归构造下一个数字。
- step 3：然后要检查每次的数字是否合法（不超过255且没有前导0）.
- step 4：合法IP需要将其连接，同时递归完这一轮后需要回溯。

**图示：**

![alt](https://uploadfiles.nowcoder.com/images/20220330/397721558_1648643386203/0EF05BBD11A798D5A7E5201B73F6430E)

**Java实现代码：**
```java
import java.util.*;
public class Solution {
    //记录分段IP数字字符串
    private String nums = ""; 
    //step表示第几个数字，index表示字符串下标
    public void dfs(String s, ArrayList<String> res, int step, int index){ 
        //当前分割出的字符串
        String cur = ""; 
        //分割出了四个数字
        if(step == 4){ 
            //下标必须走到末尾
            if(index != s.length()) 
                return;
            res.add(nums);
        }else{
            //最长遍历3位
            for(int i = index; i < index + 3 && i < s.length(); i++){ 
                cur += s.charAt(i);
                //转数字比较
                int num = Integer.parseInt(cur); 
                String temp = nums;
                //不能超过255且不能有前导0
                if(num <= 255 && (cur.length() == 1 || cur.charAt(0) != '0')){ 
                    //添加点
                    if(step - 3 != 0) 
                        nums += cur + ".";
                    else
                        nums += cur;
                    //递归查找下一个数字
                    dfs(s, res, step + 1, i + 1); 
                    //回溯
                    nums = temp; 
                }
            }
        }
    }
    public ArrayList<String> restoreIpAddresses (String s) {
        ArrayList<String> res = new ArrayList<String>();
        dfs(s, res, 0, 0);
        return res;
    }
}
```
**C++实现代码：**
```cpp
class Solution {
private: 
    //返回答案
    vector<string> res; 
    //记录输入字符串
    string s; 
    //记录分段IP数字字符串
    string nums; 
public:
    //step表示第几个数字，index表示字符串下标
    void dfs(int step, int index){ 
        //当前分割出的字符串
        string cur = ""; 
        //分割出了四个数字
        if(step == 4){ 
            //下标必须走到末尾
            if(index != s.length()) 
                return;
            res.push_back(nums);
        }else{
            //最长遍历3位
            for(int i = index; i < index + 3 && i < s.length(); i++){ 
                cur += s[i];
                //转数字比较
                int num = stoi(cur); 
                string temp = nums;
                //不能超过255且不能有前导0
                if(num <= 255 && (cur.length() == 1 || cur[0] != '0')){ 
                    //添加点
                    if(step - 3 != 0) 
                        nums += cur + ".";
                    else
                        nums += cur;
                    //递归查找下一个数字
                    dfs(step + 1, i + 1); 
                    //回溯
                    nums = temp; 
                }
            }
        }
    }
    vector<string> restoreIpAddresses(string s) {
        this->s = s;
        dfs(0, 0);
        return res;
    }
};
```
**Python代码实现：**
```Python
class Solution:
    def __init__(self):
        self.res = []
        self.s = ""
        self.nums = ""
    #step表示第几个数字，index表示字符串下标
    def dfs(self, step: int, index: int): 
        #当前分割出的字符串
        cur = "" 
        #分割出了四个数字
        if step == 4: 
            #下标必须走到末尾
            if index != len(self.s): 
                return
            self.res.append(self.nums)
        else:
            i = index
            #最长遍历3位
            while i < index + 3 and i < len(self.s): 
                cur += self.s[i]
                #转数字比较
                num = int(cur) 
                temp = self.nums
                #不能超过255且不能有前导0
                if num <= 255 and (len(cur) == 1 or cur[0] != '0'): 
                    #添加点
                    if step - 3 != 0: 
                        self.nums += cur + "."
                    else:
                        self.nums += cur
                    #递归查找下一个数字
                    self.dfs(step + 1, i + 1) 
                    #回溯
                    self.nums = temp 
                i += 1
    def restoreIpAddresses(self , s: str) -> List[str]:
        self.s = s
        self.dfs(0, 0)
        return self.res
```

**复杂度分析：**
- 时间复杂度：$O(3^n)$，3个分枝的树型递归
- 空间复杂度：$O(n)$，递归栈深度为$n$