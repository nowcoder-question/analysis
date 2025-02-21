## 题目
[题目链接](https://www.nowcoder.com/practice/34a597ee15eb4fa2b956f4c595f03218?tpId=37&tqId=36863&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路
1. 首先需要验证IP地址和子网掩码的合法性：
   - 检查每段数字是否在0-255之间
   - 检查子网掩码的二进制格式是否符合要求（连续的1后面跟着连续的0）
2. 将合法的IP地址和子网掩码转换为32位二进制数
3. 分别将两个IP地址与子网掩码进行按位与运算
4. 比较运算结果判断是否属于同一子网

---

## 代码

```c++ []
#include <iostream>
#include <string>
#include <vector>
using namespace std;

class Solution {
public:
    vector<int> parseIP(string ip) {
        vector<int> parts;
        string num;
        for (char c : ip) {
            if (c == '.') {
                parts.push_back(stoi(num));
                num = "";
            } else {
                num += c;
            }
        }
        parts.push_back(stoi(num));
        return parts;
    }
    
    bool isValidIP(string ip) {
        try {
            vector<int> parts = parseIP(ip);
            if (parts.size() != 4) return false;
            for (int part : parts) {
                if (part < 0 || part > 255) return false;
            }
            return true;
        } catch (...) {
            return false;
        }
    }
    
    bool isValidMask(string mask) {
        if (!isValidIP(mask)) return false;
        
        vector<int> parts = parseIP(mask);
        unsigned int maskInt = 0;
        for (int part : parts) {
            maskInt = (maskInt << 8) | part;
        }
        
        // 检查是否为连续的1后跟连续的0
        unsigned int temp = ~maskInt + 1;
        return maskInt && (maskInt & temp) == temp;
    }
    
    unsigned int ipToInt(string ip) {
        vector<int> parts = parseIP(ip);
        unsigned int result = 0;
        for (int part : parts) {
            result = (result << 8) | part;
        }
        return result;
    }
    
    int checkSameSubnet(string mask, string ip1, string ip2) {
        if (!isValidMask(mask) || !isValidIP(ip1) || !isValidIP(ip2)) {
            return 1;
        }
        
        unsigned int maskInt = ipToInt(mask);
        unsigned int ip1Int = ipToInt(ip1);
        unsigned int ip2Int = ipToInt(ip2);
        
        return (ip1Int & maskInt) == (ip2Int & maskInt) ? 0 : 2;
    }
};

int main() {
    string mask, ip1, ip2;
    Solution solution;
    while (cin >> mask >> ip1 >> ip2) {
        cout << solution.checkSameSubnet(mask, ip1, ip2) << endl;
    }
    return 0;
}
```

```java []
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {
            String mask = sc.nextLine();
            String ip1 = sc.nextLine();
            String ip2 = sc.nextLine();
            System.out.println(checkSameSubnet(mask, ip1, ip2));
        }
    }
    
    private static boolean isValidIP(String ip) {
        try {
            String[] parts = ip.split("\\.");
            if (parts.length != 4) return false;
            for (String part : parts) {
                int num = Integer.parseInt(part);
                if (num < 0 || num > 255) return false;
            }
            return true;
        } catch (Exception e) {
            return false;
        }
    }
    
    private static boolean isValidMask(String mask) {
        if (!isValidIP(mask)) return false;
        int maskInt = ipToInt(mask);
        // 检查是否为连续的1后跟连续的0
        return maskInt != 0 && (maskInt & (-maskInt)) == (-maskInt & 0xFFFFFFFFL);
    }
    
    private static int ipToInt(String ip) {
        String[] parts = ip.split("\\.");
        int result = 0;
        for (String part : parts) {
            result = (result << 8) | Integer.parseInt(part);
        }
        return result;
    }
    
    private static int checkSameSubnet(String mask, String ip1, String ip2) {
        if (!isValidMask(mask) || !isValidIP(ip1) || !isValidIP(ip2)) {
            return 1;
        }
        
        int maskInt = ipToInt(mask);
        int ip1Int = ipToInt(ip1);
        int ip2Int = ipToInt(ip2);
        
        return (ip1Int & maskInt) == (ip2Int & maskInt) ? 0 : 2;
    }
}
```

```python []
def is_valid_ip(ip):
    try:
        parts = [int(x) for x in ip.split('.')]
        return len(parts) == 4 and all(0 <= x <= 255 for x in parts)
    except:
        return False

def is_valid_mask(mask):
    if not is_valid_ip(mask):
        return False
    
    # 转换为32位整数
    parts = [int(x) for x in mask.split('.')]
    mask_int = sum(part << (24-i*8) for i, part in enumerate(parts))
    
    # 检查是否为连续的1后跟连续的0
    temp = (~mask_int + 1) & ((1 << 32) - 1)  # 处理Python整数无限长的特性
    return mask_int != 0 and (mask_int & temp) == temp

def ip_to_int(ip):
    parts = [int(x) for x in ip.split('.')]
    return sum(part << (24-i*8) for i, part in enumerate(parts))

def check_same_subnet(mask, ip1, ip2):
    if not is_valid_mask(mask) or not is_valid_ip(ip1) or not is_valid_ip(ip2):
        return 1
    
    mask_int = ip_to_int(mask)
    ip1_int = ip_to_int(ip1)
    ip2_int = ip_to_int(ip2)
    
    return 0 if (ip1_int & mask_int) == (ip2_int & mask_int) else 2

# 处理输入
try:
    while True:
        mask = input().strip()
        ip1 = input().strip()
        ip2 = input().strip()
        print(check_same_subnet(mask, ip1, ip2))
except:
    pass
```

---

## 算法及复杂度
- 算法：位运算
- 时间复杂度：$\mathcal{O}(1)$ - IP地址长度固定为32位
- 空间复杂度：$\mathcal{O}(1)$ - 使用固定大小的存储空间
