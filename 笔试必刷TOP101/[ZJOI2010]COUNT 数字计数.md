## 题目
[题目链接](https://www.nowcoder.com/practice/bb1a9efa244a4c9296390686ef17b024?tpId=308&tqId=271098&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

对于大范围 $[a,b]$ 内的数字统计，需要使用数位DP的方法：

1. 将问题转化为求 $f(n)$，表示 $1$ 到 $n$ 中每个数字出现的次数
   - 则区间 $[a,b]$ 的答案为 $f(b) - f(a-1)$

2. 对于计算 $f(n)$：
   - 考虑每一位的贡献
   - 例如对于数字 $2345$：
     - 个位：每 $1000$ 个数，$0-9$ 各出现 $100$ 次
     - 十位：每 $1000$ 个数，每个数字在十位上出现 $100$ 次
     - 百位：每 $1000$ 个数，每个数字在百位上出现 $100$ 次
     - 千位：$0$ 出现 $0$ 次，$1$ 出现 $1000$ 次，$2$ 出现 $346$ 次

3. 核心思路：
   - 对每一位，计算该位上的每个数字出现次数
   - 需要考虑当前位是否受限

---

## 代码

```c++ []
#include <iostream>
#include <vector>
using namespace std;
typedef long long ll;

// 计算每个数字在1到n中出现的次数
vector<ll> count_digits(ll n) {
    vector<ll> cnt(10, 0);
    ll pos = 1;  // 位权：1, 10, 100, ...
    
    while (n >= pos) {
        ll high = n / (pos * 10);  // 高位数字
        ll curr = (n / pos) % 10;  // 当前位数字
        ll low = n % pos;          // 低位数字
        
        // 对0-9每个数字统计在当前位出现的次数
        for (int digit = 0; digit < 10; digit++) {
            // 计算当前位之前的完整循环次数
            cnt[digit] += high * pos;
            
            // 处理当前位
            if (digit < curr) {
                cnt[digit] += pos;
            } else if (digit == curr) {
                cnt[digit] += low + 1;
            }
            
            // 处理前导零的特殊情况
            if (digit == 0) {
                cnt[0] -= pos;  // 减去前导零的情况
            }
        }
        
        pos *= 10;
    }
    
    return cnt;
}

int main() {
    ll a, b;
    cin >> a >> b;
    
    // 计算[1,b]和[1,a-1]的统计结果
    vector<ll> cnt_b = count_digits(b);
    vector<ll> cnt_a = count_digits(a - 1);
    
    // 输出[a,b]区间的结果
    for (int i = 0; i < 10; i++) {
        cout << cnt_b[i] - cnt_a[i] << " ";
    }
    cout << endl;
    
    return 0;
}
```
```java []
import java.util.Scanner;

public class Main {
    // 计算每个数字在1到n中出现的次数
    static long[] countDigits(long n) {
        long[] cnt = new long[10];
        long pos = 1;  // 位权：1, 10, 100, ...
        
        while (n >= pos) {
            long high = n / (pos * 10);  // 高位数字
            long curr = (n / pos) % 10;  // 当前位数字
            long low = n % pos;          // 低位数字
            
            // 对0-9每个数字统计在当前位出现的次数
            for (int digit = 0; digit < 10; digit++) {
                // 计算当前位之前的完整循环次数
                cnt[digit] += high * pos;
                
                // 处理当前位
                if (digit < curr) {
                    cnt[digit] += pos;
                } else if (digit == curr) {
                    cnt[digit] += low + 1;
                }
                
                // 处理前导零的特殊情况
                if (digit == 0) {
                    cnt[0] -= pos;  // 减去前导零的情况
                }
            }
            
            pos *= 10;
        }
        
        return cnt;
    }
    
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        long a = sc.nextLong();
        long b = sc.nextLong();
        
        // 计算[1,b]和[1,a-1]的统计结果
        long[] cnt_b = countDigits(b);
        long[] cnt_a = countDigits(a - 1);
        
        // 输出[a,b]区间的结果
        for (int i = 0; i < 10; i++) {
            System.out.print(cnt_b[i] - cnt_a[i] + " ");
        }
        System.out.println();
        
        sc.close();
    }
}
```
```python []
def count_digits(n):
    cnt = [0] * 10
    pos = 1  # 位权：1, 10, 100, ...
    
    while n >= pos:
        high = n // (pos * 10)  # 高位数字
        curr = (n // pos) % 10  # 当前位数字
        low = n % pos          # 低位数字
        
        # 对0-9每个数字统计在当前位出现的次数
        for digit in range(10):
            # 计算当前位之前的完整循环次数
            cnt[digit] += high * pos
            
            # 处理当前位
            if digit < curr:
                cnt[digit] += pos
            elif digit == curr:
                cnt[digit] += low + 1
            
            # 处理前导零的特殊情况
            if digit == 0:
                cnt[0] -= pos  # 减去前导零的情况
        
        pos *= 10
    
    return cnt

# 读取输入
a, b = map(int, input().split())

# 计算[1,b]和[1,a-1]的统计结果
cnt_b = count_digits(b)
cnt_a = count_digits(a - 1)

# 输出[a,b]区间的结果
print(*[cnt_b[i] - cnt_a[i] for i in range(10)])
```

---

## 算法及复杂度
- 算法：数位统计
- 时间复杂度：$\mathcal{O}(10 \times \log{n})$ - 对每个数字，需要遍历每一位  
- 空间复杂度：$\mathcal{O}(1)$ - 只需要固定大小的数组存储结果

