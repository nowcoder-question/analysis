## 题目
[题目链接](https://www.nowcoder.com/practice/6d76dfaf92cf478b93b60dd277b94ffa?tpId=182&tqId=225755&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

这是一道网络地址处理问题，主要思路如下：

1. 问题分析：
   - CIDR是一种IP地址分类方法
   - 需要去除被其他路由完全覆盖的路由
   - 比较两个路由时需要考虑子网掩码长度
   - 输出剩余的不重复路由

2. 解决方案：
   - 将IP地址转换为32位整数便于比较
   - 使用位运算处理子网掩码
   - 比较两个路由的网络地址部分
   - 标记被覆盖的路由并最后输出未被覆盖的路由

3. 实现细节：
   - 使用位运算优化IP地址比较
   - 正确处理子网掩码长度关系
   - 维护路由的有效性标记

---

## 代码

```cpp []
#include <iostream>
#include <vector>
using namespace std;

int main() {
    int n;
    cin >> n;
    
    // 存储IP地址和掩码
    vector<int> ip(n), mask(n);
    vector<int> flag(n, 1);  // 标记路由是否有效
    
    // 读取输入
    for(int i = 0; i < n; ++i) {
        int a, b, c, d;
        scanf("%d.%d.%d.%d/%d", &a, &b, &c, &d, &mask[i]);
        // 将IP地址转换为32位整数
        ip[i] = (a << 24) | (b << 16) | (c << 8) | d;
    }
    
    // 处理路由覆盖关系
    int res = n;
    for(int i = 0; i < n-1; ++i) {
        for(int j = i+1; j < n; ++j) {
            if(!flag[j]) continue;
            
            if(mask[i] <= mask[j]) {
                // 比较网络地址部分
                int a = ip[i] >> (32-mask[i]);
                int b = ip[j] >> (32-mask[i]);
                if(a == b) {
                    flag[j] = 0;  // j被i覆盖
                    --res;
                }
            } else {
                int a = ip[i] >> (32-mask[j]);
                int b = ip[j] >> (32-mask[j]);
                if(a == b) {
                    flag[i] = 0;  // i被j覆盖
                    --res;
                    break;
                }
            }
        }
    }
    
    // 输出结果
    cout << res << endl;
    int t = 0xFF;
    for(int i = 0; i < n; ++i) {
        if(flag[i]) {
            int temp = ip[i];
            int d = temp & t;
            temp >>= 8;
            int c = temp & t;
            temp >>= 8;
            int b = temp & t;
            temp >>= 8;
            int a = temp & t;
            printf("%d.%d.%d.%d/%d\n", a, b, c, d, mask[i]);
        }
    }
    return 0;
}
```

```java []
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        
        int[] ip = new int[n];
        int[] mask = new int[n];
        boolean[] flag = new boolean[n];
        Arrays.fill(flag, true);
        
        // 读取输入
        for(int i = 0; i < n; i++) {
            String[] parts = sc.next().split("[./]");
            ip[i] = (Integer.parseInt(parts[0]) << 24) |
                    (Integer.parseInt(parts[1]) << 16) |
                    (Integer.parseInt(parts[2]) << 8) |
                    Integer.parseInt(parts[3]);
            mask[i] = Integer.parseInt(parts[4]);
        }
        
        // 处理路由覆盖关系
        int res = n;
        for(int i = 0; i < n-1; i++) {
            for(int j = i+1; j < n; j++) {
                if(!flag[j]) continue;
                
                if(mask[i] <= mask[j]) {
                    int a = ip[i] >>> (32-mask[i]);
                    int b = ip[j] >>> (32-mask[i]);
                    if(a == b) {
                        flag[j] = false;
                        --res;
                    }
                } else {
                    int a = ip[i] >>> (32-mask[j]);
                    int b = ip[j] >>> (32-mask[j]);
                    if(a == b) {
                        flag[i] = false;
                        --res;
                        break;
                    }
                }
            }
        }
        
        // 输出结果
        System.out.println(res);
        for(int i = 0; i < n; i++) {
            if(flag[i]) {
                int temp = ip[i];
                System.out.printf("%d.%d.%d.%d/%d\n",
                    (temp >>> 24) & 0xFF,
                    (temp >>> 16) & 0xFF,
                    (temp >>> 8) & 0xFF,
                    temp & 0xFF,
                    mask[i]);
            }
        }
    }
}
```

```python []
def ip_to_int(ip_str):
    parts = list(map(int, ip_str.split('.')))
    return (parts[0] << 24) | (parts[1] << 16) | (parts[2] << 8) | parts[3]

def int_to_ip(num):
    return f"{(num >> 24) & 0xFF}.{(num >> 16) & 0xFF}.{(num >> 8) & 0xFF}.{num & 0xFF}"

def main():
    n = int(input())
    ip = []
    mask = []
    flag = [True] * n
    
    # 读取输入
    for _ in range(n):
        line = input().strip()
        addr, m = line.split('/')
        ip.append(ip_to_int(addr))
        mask.append(int(m))
    
    # 处理路由覆盖关系
    res = n
    for i in range(n-1):
        for j in range(i+1, n):
            if not flag[j]:
                continue
            
            if mask[i] <= mask[j]:
                a = ip[i] >> (32-mask[i])
                b = ip[j] >> (32-mask[i])
                if a == b:
                    flag[j] = False
                    res -= 1
            else:
                a = ip[i] >> (32-mask[j])
                b = ip[j] >> (32-mask[j])
                if a == b:
                    flag[i] = False
                    res -= 1
                    break
    
    # 输出结果
    print(res)
    for i in range(n):
        if flag[i]:
            print(f"{int_to_ip(ip[i])}/{mask[i]}")

if __name__ == "__main__":
    main()
```

---

## 算法及复杂度
- 算法：位运算 + 双重循环比较
- 时间复杂度：$\mathcal{O}(n^2)$ - $n$ 为路由数量
- 空间复杂度：$\mathcal{O}(n)$ - 存储路由信息
