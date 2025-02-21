## 题目
[题目链接](https://www.nowcoder.com/practice/66ca0e28f90c42a196afd78cc9c496ea?tpId=37&tqId=36857&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

这道题目需要实现两个转换：
1. IP地址转整数：
   - 将IP地址按点分割成4个数字
   - 每个数字转换成8位二进制
   - 将4个8位二进制数拼接成32位整数

2. 整数转IP地址：
   - 将32位整数每8位分割
   - 每8位转换成十进制数
   - 用点号连接4个十进制数

---

## 代码

```c++
#include <iostream>
#include <string>
#include <vector>
using namespace std;

// IP转整数
long ipToInt(string ip) {
    vector<int> parts;
    string temp = "";
    // 分割IP地址
    for(char c : ip) {
        if(c == '.') {
            parts.push_back(stoi(temp));
            temp = "";
        } else {
            temp += c;
        }
    }
    parts.push_back(stoi(temp));
    
    // 转换为整数
    long result = 0;
    for(int part : parts) {
        result = (result << 8) + part;
    }
    return result;
}

// 整数转IP
string intToIp(long num) {
    string result = "";
    // 从高位到低位，每8位转换成一个数字
    for(int i = 3; i >= 0; i--) {
        int part = (num >> (i * 8)) & 255;
        result += to_string(part);
        if(i > 0) result += ".";
    }
    return result;
}

int main() {
    string ip;
    long num;
    
    getline(cin, ip);
    cin >> num;
    
    cout << ipToInt(ip) << endl;
    cout << intToIp(num) << endl;
    
    return 0;
}
```

```python
def ip_to_int(ip):
    # 分割IP地址
    parts = [int(x) for x in ip.split('.')]
    # 转换为整数
    result = 0
    for part in parts:
        result = (result << 8) + part
    return result

def int_to_ip(num):
    # 从高位到低位，每8位转换成一个数字
    parts = []
    for i in range(3, -1, -1):
        part = (num >> (i * 8)) & 255
        parts.append(str(part))
    return '.'.join(parts)

if __name__ == "__main__":
    ip = input()
    num = int(input())
    
    print(ip_to_int(ip))
    print(int_to_ip(num))
```

```java
import java.util.*;

public class Main {
    // IP转整数
    public static long ipToInt(String ip) {
        String[] parts = ip.split("\\.");
        long result = 0;
        for(String part : parts) {
            result = (result << 8) + Integer.parseInt(part);
        }
        return result;
    }
    
    // 整数转IP
    public static String intToIp(long num) {
        StringBuilder result = new StringBuilder();
        for(int i = 3; i >= 0; i--) {
            result.append((num >> (i * 8)) & 255);
            if(i > 0) result.append(".");
        }
        return result.toString();
    }
    
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String ip = sc.nextLine();
        long num = sc.nextLong();
        
        System.out.println(ipToInt(ip));
        System.out.println(intToIp(num));
    }
}
```

---

## 算法及复杂度
- 算法：位运算 + 字符串处理
- 时间复杂度：$\mathcal{O}(1)$，因为IP地址长度是固定的
- 空间复杂度：$\mathcal{O}(1)$，只需要常数空间
