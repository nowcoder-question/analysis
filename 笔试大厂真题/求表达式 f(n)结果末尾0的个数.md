## 题目
[题目链接](https://www.nowcoder.com/practice/b6b63d3c0ff140a481b4f9acda922503?tpId=182&tqId=363028&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

这是一道数学题，主要思路如下：

1. 问题分析：
   - 计算 $f(n) = 1! \times 2! \times 3! \times ... \times n!$ 末尾有多少个连续的0
   - 末尾的0来源于5的因子（2的因子总是充足的）
   - 需要统计每个数中5的因子的个数

2. 解决方案：
   - 对于每个大于等于5的数，计算其中5的因子个数
   - 使用除法统计：先除以5，再除以25，再除以125...
   - 累加所有数的5的因子个数

3. 实现细节：
   - 使用辅助函数计算单个数的5的因子个数
   - 从5开始遍历到 $n$
   - 小于5的数不会贡献0

---

## 代码

```cpp []
#include <iostream>
using namespace std;

int countFives(int s) {
    int t = 0;
    while(s > 0) {
        t += s/5;
        s = s/5;
    }
    return t;
}

int main() {
    int n;
    cin >> n;
    
    int count = 0;
    if(n > 4) {
        for(int i = 5; i <= n; i++) {
            count += countFives(i);
        }
    }
    
    cout << count << endl;
    return 0;
}
```

```java []
import java.util.Scanner;

public class Main {
    public static int countFives(int s) {
        int t = 0;
        while(s > 0) {
            t += s/5;
            s = s/5;
        }
        return t;
    }
    
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        
        int count = 0;
        if(n > 4) {
            for(int i = 5; i <= n; i++) {
                count += countFives(i);
            }
        }
        
        System.out.println(count);
        sc.close();
    }
}
```

```python []
def count_fives(s: int) -> int:
    t = 0
    while s > 0:
        t += s//5
        s = s//5
    return t

def main():
    n = int(input())
    
    count = 0
    if n > 4:
        for i in range(5, n+1):
            count += count_fives(i)
    
    print(count)

if __name__ == "__main__":
    main()
```

---

## 算法及复杂度
- 算法：数学方法
- 时间复杂度：$\mathcal{O}(n \log n)$ - 需要遍历 $n$ 个数，每个数需要 $\log_5(n)$ 次除法
- 空间复杂度：$\mathcal{O}(1)$ - 只需要常数空间
