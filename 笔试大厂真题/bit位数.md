## 题目
[题目链接](https://www.nowcoder.com/practice/daf9032926614dab91ca624a7759a868?tpId=182&tqId=314260&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

这是一个位运算问题，可以通过以下步骤解决：

1. 使用异或运算(XOR)：
   - 两个数字异或后，相同位为 $0$，不同位为 $1$
   - 只需要统计异或结果中 $1$ 的个数

2. 统计1的个数方法：
   - 使用位运算 $n \& (n-1)$ 消除最低位的 $1$
   - 或使用内置函数如 $\text{__builtin_popcount}$

---

## 代码

```c++ []
#include <iostream>
using namespace std;

int countBits(int n) {
    int count = 0;
    while(n) {
        n &= (n-1);  // 消除最低位的1
        count++;
    }
    return count;
}

int main() {
    int m, n;
    cin >> m >> n;
    
    // 异或后统计1的个数
    cout << countBits(m ^ n) << endl;
    return 0;
}
```
```java []
import java.util.*;

public class Main {
    public static int countBits(int n) {
        int count = 0;
        while(n != 0) {
            n &= (n-1);  // 消除最低位的1
            count++;
        }
        return count;
    }
    
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int m = sc.nextInt();
        int n = sc.nextInt();
        
        // 异或后统计1的个数
        System.out.println(countBits(m ^ n));
    }
}
```
```python []
def count_bits(n):
    count = 0
    while n:
        n &= (n-1)  # 消除最低位的1
        count += 1
    return count

def main():
    m, n = map(int, input().split())
    
    # 异或后统计1的个数
    print(count_bits(m ^ n))

if __name__ == "__main__":
    main()
```

---

## 算法及复杂度
- 算法：位运算
- 时间复杂度：$\mathcal{O}(\log n)$，其中 $n$为数字的大小
- 空间复杂度：$\mathcal{O}(1)$

