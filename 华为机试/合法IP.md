## 题目
[题目链接](https://www.nowcoder.com/practice/995b8a548827494699dc38c3e2a54ee9?tpId=37&tqId=36914&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

这是一个判断IP地址是否合法的问题。需要检查以下几个方面：

### 关键规则
1. IP地址格式：
   - 由4个部分组成，每部分8位
   - 用点分隔，如：10.137.17.1
   - 每部分都是无符号整数

2. 合法性要求：
   - 每部分范围：0~255
   - 不能有空格
   - 不能有前导零
   - 必须是4个部分

### 实现思路
1. 输入处理：
   - 按点分割字符串
   - 检查分割后的部分数量

2. 合法性检查：
   - 检查每部分是否为数字
   - 检查数值范围
   - 检查前导零

---

## 代码

```cpp []
#include <iostream>
#include <string>
#include <vector>
using namespace std;

class Solution {
public:
    bool isValidIP(string ip) {
        // 分割IP地址
        vector<string> parts;
        string part;
        for (char c : ip) {
            if (c == '.') {
                if (part.empty()) return false;
                parts.push_back(part);
                part.clear();
            } else {
                part += c;
            }
        }
        if (!part.empty()) parts.push_back(part);
        
        // 检查部分数量
        if (parts.size() != 4) return false;
        
        // 检查每个部分
        for (string& p : parts) {
            // 检查长度
            if (p.empty() || p.length() > 3) return false;
            
            // 检查前导零
            if (p.length() > 1 && p[0] == '0') return false;
            
            // 检查是否为数字
            for (char c : p) {
                if (!isdigit(c)) return false;
            }
            
            // 检查范围
            int num = stoi(p);
            if (num < 0 || num > 255) return false;
        }
        
        return true;
    }
};

int main() {
    string ip;
    while (cin >> ip) {
        Solution sol;
        cout << (sol.isValidIP(ip) ? "YES" : "NO") << endl;
    }
    return 0;
}
```

```java []
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {
            String ip = sc.next();  // 使用next()而不是nextLine()
            System.out.println(isValidIP(ip) ? "YES" : "NO");
        }
    }
    
    public static boolean isValidIP(String ip) {
        // 检查是否包含空格
        if (ip.contains(" ")) return false;
        
        // 分割IP地址
        String[] parts = ip.split("\\.");
        
        // 检查部分数量
        if (parts.length != 4) return false;
        
        // 检查每个部分
        for (String part : parts) {
            // 检查长度
            if (part.isEmpty() || part.length() > 3) return false;
            
            // 检查前导零
            if (part.length() > 1 && part.charAt(0) == '0') return false;
            
            // 检查是否为数字
            for (char c : part.toCharArray()) {
                if (!Character.isDigit(c)) return false;
            }
            
            // 检查范围
            try {
                int num = Integer.parseInt(part);
                if (num < 0 || num > 255) return false;
            } catch (NumberFormatException e) {
                return false;
            }
        }
        
        // 检查原始字符串中的点的数量（避免多余的点）
        int dotCount = 0;
        for (char c : ip.toCharArray()) {
            if (c == '.') dotCount++;
        }
        if (dotCount != 3) return false;
        
        return true;
    }
}
```

```python []
def isValidIP(ip):
    # 分割IP地址
    parts = ip.split('.')
    
    # 检查部分数量
    if len(parts) != 4:
        return False
        
    # 检查每个部分
    for part in parts:
        # 检查长度
        if not part or len(part) > 3:
            return False
            
        # 检查前导零
        if len(part) > 1 and part[0] == '0':
            return False
            
        # 检查是否为数字
        if not part.isdigit():
            return False
            
        # 检查范围
        num = int(part)
        if num < 0 or num > 255:
            return False
            
    return True

# 主程序
while True:
    try:
        ip = input().strip()
        print("YES" if isValidIP(ip) else "NO")
    except:
        break
```

---

## 算法及复杂度

### 算法分析
1. 字符串分割：
   - 按点分割字符串
   - 检查分割后的部分

2. 数值检查：
   - 检查数字有效性
   - 检查范围合法性
   - 检查格式规范

### 复杂度分析
- 时间复杂度：$\mathcal{O}(n)$ - 其中 $n$ 是IP地址的长度
- 空间复杂度：$\mathcal{O}(n)$ - 需要存储分割后的字符串
