## 题目
[题目链接](https://www.nowcoder.com/practice/e81c8d4652ea4d72abd94d8e443b8ee7?tpId=182&tqId=223150&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

这道题目可以通过异或运算的性质来解决：
1. 如果三个科室的面试官人数经过异或运算后为0，说明小招必须出差
2. 否则，我们需要找到第一个可以减少人数使得剩余异或为0的科室
3. 通过比较每个科室当前人数与异或后的人数，可以判断是否可以在该科室减少人数

---

## 代码

```c++ []
#include <iostream>
using namespace std;

int main() {
    int a, b, c;
    char c1, c2;
    cin >> a >> c1 >> b >> c2 >> c;
    
    // 计算三个数的异或值
    int k = a ^ b ^ c;
    
    // 如果异或值为0，必须出差
    if (k == 0) {
        cout << 1 << endl;
        return 0;
    }
    
    // 检查每个科室是否可以通过减少人数实现异或为0
    if (a >= (a ^ k)) {
        cout << "A," << (a - (a ^ k)) << endl;
    }
    else if (b >= (b ^ k)) {
        cout << "B," << (b - (b ^ k)) << endl;
    }
    else if (c >= (c ^ k)) {
        cout << "C," << (c - (c ^ k)) << endl;
    }
    return 0;
}
```
```java []
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String[] input = sc.nextLine().split(",");
        int a = Integer.parseInt(input[0]);
        int b = Integer.parseInt(input[1]);
        int c = Integer.parseInt(input[2]);
        
        int k = a ^ b ^ c;
        
        if (k == 0) {
            System.out.println(1);
            return;
        }
        
        if (a >= (a ^ k)) {
            System.out.println("A," + (a - (a ^ k)));
        }
        else if (b >= (b ^ k)) {
            System.out.println("B," + (b - (b ^ k)));
        }
        else if (c >= (c ^ k)) {
            System.out.println("C," + (c - (c ^ k)));
        }
    }
}
```
```python []
a, b, c = map(int, input().split(','))

# 计算异或值
k = a ^ b ^ c

# 如果异或值为0，必须出差
if k == 0:
    print(1)
else:
    # 检查每个科室是否可以通过减少人数实现异或为0
    if a >= (a ^ k):
        print(f"A,{a - (a ^ k)}")
    elif b >= (b ^ k):
        print(f"B,{b - (b ^ k)}")
    elif c >= (c ^ k):
        print(f"C,{c - (c ^ k)}")
```

---

## 算法及复杂度
- 算法：使用异或运算和贪心策略
- 时间复杂度：$\mathcal{O}(1)$ - 只需要进行简单的异或运算和比较
- 空间复杂度：$\mathcal{O}(1)$ - 只使用了常数级别的额外空间
