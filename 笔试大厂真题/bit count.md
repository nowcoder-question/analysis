## 题目
[题目链接](https://www.nowcoder.com/practice/bc4c7936f5ed42cbb9131b6f39aa272b?tpId=182&tqId=365882&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

这是一道位运算题目，主要思路如下：

1. 问题分析：
   - 输入一个long类型的数值
   - 需要计算其二进制表示中1的个数
   - 需要处理负数的情况

2. 解决方案：
   - 方法一：循环右移，统计最低位1
   - 方法二：n & (n-1)消除最低位1
   - 方法三：查表法统计每8位中1的个数
   - 方法四：内置函数（如果允许使用）

3. 实现细节：
   - 注意处理负数情况
   - 考虑不同语言的整数范围
   - 选择高效的统计方法

---

## 代码

```cpp []
#include <iostream>
using namespace std;

int countBits(long n) {
    int count = 0;
    // 使用n&(n-1)消除最低位的1
    while (n) {
        n &= (n - 1);
        count++;
    }
    return count;
}

int main() {
    long n;
    while (cin >> n) {
        cout << countBits(n) << endl;
    }
    return 0;
}
```

```java []
import java.util.Scanner;

public class Main {
    public static int countBits(long n) {
        int count = 0;
        // Java中long是64位的，需要遍历所有位
        while (n != 0) {
            n &= (n - 1);  // 消除最低位的1
            count++;
        }
        return count;
    }
    
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (sc.hasNextLong()) {
            long n = sc.nextLong();
            System.out.println(countBits(n));
        }
        sc.close();
    }
}
```

```python []
def count_bits(n):
    # Python的整数没有位数限制，但我们只需要统计到64位
    # 将负数转换为无符号数处理
    if n < 0:
        n = n & ((1 << 64) - 1)
    count = 0
    while n:
        n &= (n - 1)  # 消除最低位的1
        count += 1
    return count

def main():
    while True:
        try:
            n = int(input())
            print(count_bits(n))
        except EOFError:
            break

if __name__ == "__main__":
    main()
```

---

## 算法及复杂度
- 算法：位运算
- 时间复杂度：$\mathcal{O}(k)$ - $k$ 为二进制中1的个数
- 空间复杂度：$\mathcal{O}(1)$ - 只需要常数额外空间