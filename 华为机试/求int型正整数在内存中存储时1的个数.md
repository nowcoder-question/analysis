## 题目
[题目链接](https://www.nowcoder.com/practice/440f16e490a0404786865e99c6ad91c9?tpId=37&tqId=36839&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路
1. 将整数转换为二进制表示
2. 统计二进制表示中1的个数
3. 可以使用以下方法：
   - 方法1：使用位运算逐位检查
   - 方法2：使用语言内置函数

---

## 代码

```c++ []
#include <iostream>
using namespace std;

int main() {
    int n;
    cin >> n;
    
    // cout << __builtin_popcount(n) << endl;
    int count = 0;
    while (n > 0) {
        count += n & 1;  // 检查最低位是否为1
        n >>= 1;         // 右移一位
    }
    
    cout << count << endl;
    return 0;
}
```
```java []
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        
        int count = 0;
        while (n > 0) {
            count += n & 1;  // 检查最低位是否为1
            n >>= 1;         // 右移一位
        }
        
        System.out.println(count);
    }
}
```
```python []
n = int(input())

# 方法1：使用bin函数
print(bin(n).count('1'))

'''
# 方法2：位运算
count = 0
while n > 0:
    count += n & 1
    n >>= 1

print(count)
'''
```

---

## 算法及复杂度
- 算法：位运算或字符串处理
- 时间复杂度：$\mathcal{O}(b)$ - 其中b为二进制位数
- 空间复杂度：$\mathcal{O}(1)$ - 只需常数级空间
