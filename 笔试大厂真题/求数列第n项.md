## 题目
[题目链接](https://www.nowcoder.com/practice/d15363742fe94a0ea4030e5124713fac?tpId=182&tqId=325928&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

观察数列 $1, 2, 3, 3, 4, 4, 4, 5, 5, 5, 5, 5...$，可以发现：
1. 每个数字 $i$ 会重复 $i$ 次
2. 要找到第 $n$ 个数所在层之前的所有数的和，需要：
   - 使用斐波那契数列来计算每层的累积和
   - 当累积和大于等于 $n$ 时，减去最后一个数即为答案

---

## 代码

```cpp []
#include <iostream>
using namespace std;

int main() {
    long long n, a = 0, b = 1, c, sum = 1;
    cin >> n;
    while (sum < n) {
        c = a + b;
        a = b;
        b = c;
        sum += c;
    }
    cout << sum - b << endl;
    return 0;
}
```
```java []
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        long n = sc.nextLong();
        long a = 0, b = 1, sum = 1;
        while (sum < n) {
            long c = a + b;
            a = b;
            b = c;
            sum += c;
        }
        System.out.println(sum - b);
    }
}
```
```python []
n = int(input())
a, b = 0, 1
sum = 1
while sum < n:
    c = a + b
    a = b
    b = c
    sum += c
print(sum - b)
```

---

## 算法及复杂度
- 算法：斐波那契数列  
- 时间复杂度：$\mathcal{O}(\log n)$ - 因为斐波那契数列增长速度很快，循环次数与 $n$ 的对数相关  
- 空间复杂度：$\mathcal{O}(1)$ - 只使用了常数个变量
