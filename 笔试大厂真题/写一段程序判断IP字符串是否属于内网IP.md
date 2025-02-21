## 题目
[题目链接](https://www.nowcoder.com/practice/80ce674313ff43af9d7ac7a41ae21527?tpId=182&tqId=362283&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

这是一道IP地址判断题目，需要判断一个IP是否为内网IP。内网IP的规则如下：

1. 以127开头的IP地址（127.0.0.0/8）
2. 以10开头的IP地址（10.0.0.0/8）
3. 以192.168开头的IP地址（192.168.0.0/16）
4. 以172.16到172.31开头的IP地址（172.16.0.0/12）

---

## 代码

```cpp []
#include <iostream>
using namespace std;

int main() {
    int p1, p2, p3, p4;
    
    // 使用scanf按点分十进制格式读取IP地址
    scanf("%d.%d.%d.%d", &p1, &p2, &p3, &p4);
    
    // 判断是否为内网IP
    if(p1 == 127) {  // 127.0.0.0/8
        printf("1");
    }
    else if(p1 == 10) {  // 10.0.0.0/8
        printf("1");
    }
    else if(p1 == 192 && p2 == 168) {  // 192.168.0.0/16
        printf("1");
    }
    else if(p1 == 172 && p2 >= 16 && p2 <= 31) {  // 172.16.0.0/12
        printf("1");
    }
    else {
        printf("0");
    }
    
    return 0;
}
```

```java []
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        
        // 读取IP地址字符串并分割
        String[] parts = sc.next().split("\\.");
        int p1 = Integer.parseInt(parts[0]);
        int p2 = Integer.parseInt(parts[1]);
        int p3 = Integer.parseInt(parts[2]);
        int p4 = Integer.parseInt(parts[3]);
        
        // 判断是否为内网IP
        if(p1 == 127) {  // 127.0.0.0/8
            System.out.println("1");
        }
        else if(p1 == 10) {  // 10.0.0.0/8
            System.out.println("1");
        }
        else if(p1 == 192 && p2 == 168) {  // 192.168.0.0/16
            System.out.println("1");
        }
        else if(p1 == 172 && p2 >= 16 && p2 <= 31) {  // 172.16.0.0/12
            System.out.println("1");
        }
        else {
            System.out.println("0");
        }
    }
}
```

```python []
def is_private_ip(ip):
    # 分割IP地址
    parts = list(map(int, ip.split('.')))
    p1, p2, p3, p4 = parts
    
    # 判断是否为内网IP
    if p1 == 127:  # 127.0.0.0/8
        return True
    elif p1 == 10:  # 10.0.0.0/8
        return True
    elif p1 == 192 and p2 == 168:  # 192.168.0.0/16
        return True
    elif p1 == 172 and 16 <= p2 <= 31:  # 172.16.0.0/12
        return True
    return False

def main():
    # 读取IP地址
    ip = input()
    
    # 输出结果
    print("1" if is_private_ip(ip) else "0")

if __name__ == "__main__":
    main()
```

---

## 算法及复杂度
- 算法：条件判断
- 时间复杂度：$\mathcal{O}(1)$ - 只需要简单的条件判断
- 空间复杂度：$\mathcal{O}(1)$ - 只需要存储IP地址的四个部分
