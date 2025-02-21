## 题目
[题目链接](https://www.nowcoder.com/practice/97142035f7d2443c91a3ffc343ad691d?tpId=182&tqId=353487&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

这是一道数论题目，主要思路如下：

1. N-GCD的定义：
   - 对于一组数对 $(a_i, b_i)$，如果存在一个质数 $x$
   - $x$ 可以整除每个数对中的至少一个数
   - 且 $x$ 大于1，则 $x$ 就是这些数对的 N-GCD

2. 求解步骤：
   - 找出所有数对中最小的数
   - 生成不超过这个最小数的所有质数
   - 检查每个质数是否满足N-GCD的条件
   - 输出最大的满足条件的质数

---

## 代码

```cpp []
#include <iostream>
#include <vector>
#include <cmath>
using namespace std;

// 判断是否为质数
bool isPrime(int num) {
    if(num <= 3) return num > 1;
    if(num % 6 != 1 && num % 6 != 5) return false;
    
    int sqrtNum = sqrt(num);
    for(int i = 5; i <= sqrtNum; i += 6) {
        if(num % i == 0 || num % (i + 2) == 0) return false;
    }
    return true;
}

int main() {
    int n;
    cin >> n;
    
    // 读取数对并找出最小值
    vector<pair<int, int>> pairs(n);
    int minVal = 2e9;
    
    for(int i = 0; i < n; i++) {
        cin >> pairs[i].first >> pairs[i].second;
        minVal = min(minVal, min(pairs[i].first, pairs[i].second));
    }
    
    // 生成质数列表
    vector<int> primes;
    for(int i = 2; i <= minVal; i++) {
        if(isPrime(i)) primes.push_back(i);
    }
    
    // 检查每个质数是否满足N-GCD条件
    int result = -1;
    for(int prime : primes) {
        bool isValid = true;
        for(const auto& p : pairs) {
            if(p.first % prime != 0 && p.second % prime != 0) {
                isValid = false;
                break;
            }
        }
        if(isValid) result = prime;
    }
    
    cout << result << endl;
    return 0;
}
```

```java []
import java.util.*;

public class Main {
    // 判断是否为质数
    static boolean isPrime(int num) {
        if(num <= 3) return num > 1;
        if(num % 6 != 1 && num % 6 != 5) return false;
        
        int sqrtNum = (int)Math.sqrt(num);
        for(int i = 5; i <= sqrtNum; i += 6) {
            if(num % i == 0 || num % (i + 2) == 0) return false;
        }
        return true;
    }
    
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        
        // 读取数对并找出最小值
        int[][] pairs = new int[n][2];
        int minVal = Integer.MAX_VALUE;
        
        for(int i = 0; i < n; i++) {
            pairs[i][0] = sc.nextInt();
            pairs[i][1] = sc.nextInt();
            minVal = Math.min(minVal, Math.min(pairs[i][0], pairs[i][1]));
        }
        
        // 生成质数列表
        ArrayList<Integer> primes = new ArrayList<>();
        for(int i = 2; i <= minVal; i++) {
            if(isPrime(i)) primes.add(i);
        }
        
        // 检查每个质数是否满足N-GCD条件
        int result = -1;
        for(int prime : primes) {
            boolean isValid = true;
            for(int[] pair : pairs) {
                if(pair[0] % prime != 0 && pair[1] % prime != 0) {
                    isValid = false;
                    break;
                }
            }
            if(isValid) result = prime;
        }
        
        System.out.println(result);
    }
}
```

```python []
from math import sqrt

def is_prime(num):
    """判断是否为质数"""
    if num <= 3:
        return num > 1
    if num % 6 != 1 and num % 6 != 5:
        return False
    
    sqrt_num = int(sqrt(num))
    for i in range(5, sqrt_num + 1, 6):
        if num % i == 0 or num % (i + 2) == 0:
            return False
    return True

def main():
    n = int(input())
    
    # 读取数对并找出最小值
    pairs = []
    min_val = float('inf')
    
    for _ in range(n):
        a, b = map(int, input().split())
        pairs.append((a, b))
        min_val = min(min_val, min(a, b))
    
    # 生成质数列表
    primes = [i for i in range(2, min_val + 1) if is_prime(i)]
    
    # 检查每个质数是否满足N-GCD条件
    result = -1
    for prime in primes:
        if all(a % prime == 0 or b % prime == 0 for a, b in pairs):
            result = prime
    
    print(result)

if __name__ == "__main__":
    main()
```

---

## 算法及复杂度
- 算法：数论 + 质数筛选
- 时间复杂度：$\mathcal{O}(n \times \sqrt{m})$ - 其中 $n$ 为数对个数，$m$ 为最小值
- 空间复杂度：$\mathcal{O}(m)$ - 需要存储质数列表
