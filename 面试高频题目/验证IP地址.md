## 题目
[题目链接](https://www.nowcoder.com/practice/55fb3c68d08d46119f76ae2df7566880?tpId=196&tqId=1024725&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

##### 题目主要信息：

- IPv4只有十进制数和分割点，其中数字在0-255之间，共4组，且不能有零开头的非零数，不能缺省
- IPv6由8组16进制数组成，会出现a-fA-F，通过冒号分割，不可缺省，可以零开头，或者为一个单独零，每组最多4位。

##### 举一反三：

学会了本题的思路，你将可以解决类似的字符串问题：

[BM83. 字符串变形](https://www.nowcoder.com/practice/c3120c1c1bc44ad986259c0cf0f0b80e?tpId=295&sfm=html&channel=nowcoder)

[BM84. 最长公共前缀](https://www.nowcoder.com/practice/28eb3175488f4434a4a6207f6f484f47?tpId=295&sfm=html&channel=nowcoder)

##### 方法一：分割字符串比较法（推荐使用）

**思路：**

我们可以先对IP字符串进行分割，然后依次判断每个分割是否符合要求。

**具体做法：**

- step 1：写一个split函数（或者内置函数）。
- step 2：遍历IP字符串，遇到.或者:将其分开储存在一个数组中。
- step 3：遍历数组，对于IPv4，需要依次验证分组为4，分割不能缺省，没有前缀0或者其他字符，数字在0-255范围内。
- step 4：对于IPv6，需要依次验证分组为8，分割不能缺省，每组不能超过4位，不能出现除数字小大写a-f以外的字符。

**图示：**

![图片说明](https://uploadfiles.nowcoder.com/images/20210721/397721558_1626858977926/738B9E9B9C66144EE1BF52477AB94768 "图片标题") 

**java代码实现:**
```java
import java.util.*;
public class Solution {
    boolean isIPv4 (String IP) {
        if(IP.indexOf('.') == -1){
            return false;
        }
        String[] s = IP.split("\\.");  
        //IPv4必定为4组
        if(s.length != 4)  
            return false;
        for(int i = 0; i < s.length; i++){
            //不可缺省，有一个分割为零，说明两个点相连
            if(s[i].length() == 0)  
                return false;
            //比较数字位数及不为零时不能有前缀零
            if(s[i].length() < 0 || s[i].length() > 3 || (s[i].charAt(0)=='0' && s[i].length() != 1))  
                return false;
            int num = 0;
            //遍历每个分割字符串，必须为数字
            for(int j = 0; j < s[i].length(); j++){  
                char c = s[i].charAt(j);
                if (c < '0' || c > '9') 
                    return false;
            //转化为数字比较，0-255之间
            num = num * 10 + (int)(c - '0');  
            if(num < 0 || num > 255) 
                return false;
            }
        }    
        return true;
    }
    boolean isIPv6 (String IP) {
        if (IP.indexOf(':') == -1) {
            return false;
        }
        String[] s = IP.split(":",-1);
        //IPv6必定为8组
        if(s.length != 8){  
            return false;
        }
        for(int i = 0; i < s.length; i++){ 
            //每个分割不能缺省，不能超过4位
            if(s[i].length() == 0 || s[i].length() > 4){ 
                return false; 
            }
            for(int j = 0; j < s[i].length(); j++){
                //不能出现a-fA-F以外的大小写字符
                char c = s[i].charAt(j);
                boolean expr = (c >= '0' && c <= '9') || (c >= 'a' && c <= 'f') || (c >= 'A' && c <= 'F') ;
                if(!expr){
                    return false;
                }
            }
        }
        return true;
    }
    String solve(String IP) {
        if(isIPv4(IP))
            return "IPv4";
        else if(isIPv6(IP))
            return "IPv6";
        return "Neither";
    }
}
```
**c++代码实现:**
```cpp
class Solution {
public:
    //将字符串从.或者:分割开
    vector<string> split(string s, string spliter) {  
        vector<string> res;
        int i;
        //遍历字符串查找spliter
        while((i = s.find(spliter)) && i != s.npos){  
            //将分割的部分加入vector中
            res.push_back(s.substr(0, i));  
            s = s.substr(i + 1);
        }
        res.push_back(s);
        return res;
    }
    bool isIPv4 (string IP) {
        vector<string> s = split(IP, ".");  
        //IPv4必定为4组
        if(s.size() != 4)  
            return false;
        for(int i = 0; i < s.size(); i++){
            //不可缺省，有一个分割为零，说明两个点相连
            if (s[i].size() == 0)  
                return false;
            //比较数字位数及不为零时不能有前缀零
            if (s[i].size() < 0 || s[i].size() > 3 || (s[i][0]=='0' && s[i].size() != 1))  
                return false;
            //遍历每个分割字符串，必须为数字
            for (int j = 0; j < s[i].size(); j++)  
                if (!isdigit(s[i][j])) 
                    return false;
            //转化为数字比较，0-255之间
            int num = stoi(s[i]);  
            if (num < 0 || num > 255) 
                return false;
        }    
        return true;
    }
    bool isIPv6 (string IP) {
        vector<string> s = split(IP, ":");
        //IPv6必定为8组
        if(s.size() != 8)  
            return false;
        for(int i = 0; i < s.size(); i++){ 
            //每个分割不能缺省，不能超过4位
            if(s[i].size() == 0 || s[i].size() > 4)  
                return false; 
            for(int j = 0; j < s[i].size(); j++){
                //不能出现a-fA-F以外的大小写字符
                if(!(isdigit(s[i][j]) || (s[i][j] >= 'a' && s[i][j] <= 'f') || (s[i][j] >= 'A' && s[i][j] <= 'F')))
                    return false;
            }
        }
        return true;
    }
    string solve(string IP) {
        if(IP.size() == 0)
            return "Neither";
        if(isIPv4(IP))
            return "IPv4";
        else if(isIPv6(IP))
            return "IPv6";
        return "Neither";
    }
};
```

**Python实现代码：**
```python
class Solution:
    def isIPv4 (self, IP:str):
        s = IP.split('.')  
        #IPv4必定为4组
        if len(s)!= 4:  
            return False
        for i in range(len(s)):
            #不可缺省，有一个分割为零，说明两个点相连
            if len(s[i])==0:  
                return False
            #比较数字位数及不为零时不能有前缀零
            if len(s[i]) < 0 or len(s[i]) > 3 or (s[i][0]=='0' and len(s[i]) != 1):
                return False
            #遍历每个分割字符串，必须为数字
            for j in range(len(s[i])): 
                if s[i][j]<'0' or s[i][j]>'9':
                    return False
            #转化为数字比较，0-255之间
            num = int(s[i]) 
            if  num < 0 or num > 255:
                return False
        return True
    
    def isIPv6 (self, IP:str):
        s = IP.split(':') 
        #IPv6必定为8组
        if len(s) != 8: 
            return False
        for i in range(len(s)):
            #每个分割不能缺省，不能超过4位
            if len(s[i]) == 0 or len(s[i]) > 4:  
                return False
            for j in range(len(s[i])):
                #不能出现a-fA-F以外的大小写字符
                if not (s[i][j].isdigit() or s[i][j] >= 'a' and s[i][j] <= 'f' or s[i][j] >='A' and s[i][j] <= 'F'):
                    return False
        return True
    
    def solve(self , IP: str) -> str:
        if len(IP) == 0:
            return "Neither"
        if Solution.isIPv4(self, IP):
            return "IPv4"
        elif Solution.isIPv6(self, IP):
            return "IPv6"
        return "Neither"
```

**复杂度分析：**
- 时间复杂度：$O(n)$，n为字符串IP的长度，判断部分只遍历4组或者8组，但是分割字符串需要遍历全部
- 空间复杂度：$O(1)$，储存分割字符串的临时空间为常数4或者8


##### 方法二：正则表达式（扩展思路）

**思路：**

IP地址是有规律可言的：IPv4用了4个0-255的数字，用点隔开，IPv6用了4位十六进制的数字，用冒号隔开，共8组，这都可以直接用正则表达式来表示。

**具体做法：**

- step 1：IPv4的正则表达式限制数字在0-255，且没有前缀0，用3个点隔开共4组。
- step 2：IPv6的正则表达式限制出现8组，0-9a-fA-F的数，个数必须是1-4个，用冒号隔开。
- step 3：调用函数依次比较给定字符串与模板串之间是否匹配，匹配哪一个就属于哪一种IP地址，否则都不是。

**Java代码实现:**
```java
import java.util.regex.Pattern;
public class Solution {
    String solve(String IP) {
        //正则表达式限制0-255 且没有前缀0 四组齐全
        String ipv4="(([0-9]|[1-9][0-9]|1[0-9][0-9]|2[0-4][0-9]|25[0-5])\\.){3}([0-9]|[1-9][0-9]|1[0-9][0-9]|2[0-4][0-9]|25[0-5])";
        Pattern ipv4_pattern = Pattern.compile(ipv4);
        //正则表达式限制出现8组，0-9a-fA-F的数，个数必须是1-4个
        String ipv6="([0-9a-fA-F]{1,4}\\:){7}[0-9a-fA-F]{1,4}";
        Pattern ipv6_pattern = Pattern.compile(ipv6);
        //调用正则匹配函数
        if (ipv4_pattern.matcher(IP).matches()) 
            return "IPv4";
        else if (ipv6_pattern.matcher(IP).matches()) 
            return "IPv6";
        else return "Neither";
    }
}
```

**C++代码实现:**
```cpp
//必须加正则表达式的库
#include <regex>  
class Solution {
public:
    string solve(string IP) {
        //正则表达式限制0-255 且没有前缀0 四组齐全
        regex ipv4("(([0-9]|[1-9][0-9]|1[0-9][0-9]|2[0-4][0-9]|25[0-5])\\.){3}([0-9]|[1-9][0-9]|1[0-9][0-9]|2[0-4][0-9]|25[0-5])");
        //正则表达式限制出现8组，0-9a-fA-F的数，个数必须是1-4个
        regex ipv6("([0-9a-fA-F]{1,4}\\:){7}[0-9a-fA-F]{1,4}");
        //调用正则匹配函数
        if (regex_match(IP, ipv4)) 
            return "IPv4";
        else if (regex_match(IP, ipv6)) 
            return "IPv6";
        else return "Neither";
    }
};
```

**Python实现代码：**
```python
import re
class Solution:
    def solve(self , IP: str) -> str:
         #正则表达式限制0-255 且没有前缀0 四组齐全
        ipv4 = "(([0-9]|[1-9][0-9]|1[0-9][0-9]|2[0-4][0-9]|25[0-5])\.){3}([0-9]|[1-9][0-9]|1[0-9][0-9]|2[0-4][0-9]|25[0-5])"
        #正则表达式限制出现8组，0-9a-fA-F的数，个数必须是1-4个
        ipv6 = "([0-9a-fA-F]{1,4}\:){7}([0-9a-fA-F]{1,4})$"
        ipv4 = re.compile(ipv4)
        ipv6 = re.compile(ipv6)
        #调用正则匹配函数
        if ipv4.match(IP): 
            return "IPv4"
        elif ipv6.match(IP):
            return "IPv6"
        else:
            return "Neither"
```
**复杂度分析：**
- 时间复杂度：$O(n)$，regex_match函数默认$O(n)$
- 空间复杂度：$O(1)$，没有使用额外空间