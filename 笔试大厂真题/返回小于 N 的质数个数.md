## 题目
[题目链接](https://www.nowcoder.com/practice/9e7a88d6a00e404c8418602515a5046c?tpId=182&tqId=177028&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

使用埃氏筛法（Eratosthenes筛法）计算质数：
1. 创建标记数组
2. 从2开始标记所有合数
3. 优化筛选范围到sqrt(n)

### 关键点
1. 使用位运算优化空间
2. 只需标记到sqrt(n)
3. 跳过偶数优化

---

## 代码
```cpp []
#include <iostream>
#include <bitset>
using namespace std;

class Solution {
private:
    static const int MAX_N = 5000005;  // 根据题目范围调整
    bitset<MAX_N> isPrime;  // 使用bitset节省空间
    
public:
    Solution() {
        // 初始化埃氏筛
        isPrime.set();  // 全部设为true
        isPrime[0] = isPrime[1] = false;
        
        for (int i = 2; i * i < MAX_N; i++) {
            if (isPrime[i]) {
                // 从i*i开始标记，因为小于i*i的合数已被标记
                for (int j = i * i; j < MAX_N; j += i) {
                    isPrime[j] = false;
                }
            }
        }
    }
    
    int countPrimes(int n) {
        int count = 0;
        for (int i = 2; i < n; i++) {
            if (isPrime[i]) count++;
        }
        return count;
    }
};

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    
    Solution solution;  // 预处理质数表
    
    int n;
    while (cin >> n) {
        cout << solution.countPrimes(n) << endl;
    }
    
    return 0;
}
```

```java []
import java.util.*;

public class Main {
    static class Solution {
        private static final int MAX_N = 5000005;
        private final boolean[] isPrime;
        
        public Solution() {
            isPrime = new boolean[MAX_N];
            Arrays.fill(isPrime, true);
            isPrime[0] = isPrime[1] = false;
            
            for (int i = 2; i * i < MAX_N; i++) {
                if (isPrime[i]) {
                    for (int j = i * i; j < MAX_N; j += i) {
                        isPrime[j] = false;
                    }
                }
            }
        }
        
        public int countPrimes(int n) {
            int count = 0;
            for (int i = 2; i < n; i++) {
                if (isPrime[i]) count++;
            }
            return count;
        }
    }
    
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        Solution solution = new Solution();  // 预处理质数表
        
        while (sc.hasNext()) {
            int n = sc.nextInt();
            System.out.println(solution.countPrimes(n));
        }
        sc.close();
    }
}
```

```python []
class Solution:
    def __init__(self):
        MAX_N = 5000005
        # 使用bytearray替代bool列表以节省空间
        self.is_prime = bytearray([1] * MAX_N)
        self.is_prime[0] = self.is_prime[1] = 0
        
        # 埃氏筛
        for i in range(2, int(MAX_N ** 0.5) + 1):
            if self.is_prime[i]:
                # 使用切片赋值优化性能
                self.is_prime[i*i:MAX_N:i] = bytearray([0] * len(range(i*i, MAX_N, i)))
    
    def count_primes(self, n):
        return sum(self.is_prime[2:n])

def main():
    solution = Solution()  # 预处理质数表
    
    while True:
        try:
            n = int(input())
            print(solution.count_primes(n))
        except EOFError:
            break

if __name__ == "__main__":
    main()
```

---

## 算法及复杂度
- 算法：埃氏筛法
- 时间复杂度：预处理 $\mathcal{O}(n \log \log n)$，查询 $\mathcal{O}(1)$
- 空间复杂度：$\mathcal{O}(n)$，使用bitset/bytearray优化

