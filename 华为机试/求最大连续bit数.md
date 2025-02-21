## 题目
[题目链接](https://www.nowcoder.com/practice/4b1658fd8ffb4217bc3b7e85a38cfaf2?tpId=37&tqId=36910&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

这是一个求二进制数中最大连续1的个数的问题。

### 解题步骤
1. 将输入的十进制数转换为二进制字符串
2. 遍历二进制字符串，统计最大连续1的个数
3. 输出结果

### 实现细节
1. 十进制转二进制：
   - 使用内置函数
   - 或者使用位运算和除2取余的方法

2. 统计连续1：
   - 遍历二进制字符串
   - 维护当前连续1的计数和最大连续1的计数
   - 遇到0时重置当前计数

---

## 代码

``` cpp []
#include <iostream>
#include <string>
using namespace std;

int maxConsecutiveOnes(int n) {
    int maxCount = 0;
    int currentCount = 0;
    
    while (n > 0) {
        if (n & 1) {  // 当前位是1
            currentCount++;
            maxCount = max(maxCount, currentCount);
        } else {  // 当前位是0
            currentCount = 0;
        }
        n >>= 1;  // 右移一位
    }
    
    return maxCount;
}

int main() {
    int n;
    while (cin >> n) {
        cout << maxConsecutiveOnes(n) << endl;
    }
    return 0;
}
```
``` java []
import java.util.Scanner;

public class Main {
    public static int maxConsecutiveOnes(int n) {
        int maxCount = 0;
        int currentCount = 0;
        
        while (n > 0) {
            if ((n & 1) == 1) {  // 当前位是1
                currentCount++;
                maxCount = Math.max(maxCount, currentCount);
            } else {  // 当前位是0
                currentCount = 0;
            }
            n >>= 1;  // 右移一位
        }
        
        return maxCount;
    }
    
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {
            int n = sc.nextInt();
            System.out.println(maxConsecutiveOnes(n));
        }
    }
}
```
``` python []
def max_consecutive_ones(n: int) -> int:
    max_count = 0
    current_count = 0
    
    while n > 0:
        if n & 1:  # 当前位是1
            current_count += 1
            max_count = max(max_count, current_count)
        else:  # 当前位是0
            current_count = 0
        n >>= 1  # 右移一位
    
    return max_count

# 处理输入
while True:
    try:
        n = int(input())
        print(max_consecutive_ones(n))
    except EOFError:
        break
```

---

## 算法及复杂度

### 算法分析
1. 使用位运算：
   - 通过 `n & 1` 判断最低位是否为1
   - 通过 `n >>= 1` 右移一位
   - 不需要实际转换为二进制字符串

2. 计数策略：
   - 遇到1时增加当前计数
   - 遇到0时重置当前计数
   - 持续更新最大计数

### 复杂度分析
- 时间复杂度：$\mathcal{O}(\log n)$ - 需要检查每个二进制位
- 空间复杂度：$\mathcal{O}(1)$ - 只需要几个变量存储计数
