## 题目
[题目链接](https://www.nowcoder.com/practice/46bd43f043c54013a67816d0a2946506?tpId=182&tqId=105230&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

1. 这是geohash编码的第一步：二进制编码过程
2. 给定一个范围 $[-90, 90]$ 内的数字 $n$，需要通过二分法将其转换为6位二进制编码
3. 算法流程：
   - 每次将区间二分，得到中点 $mid$
   - 如果目标数 $n$ 大于等于 $mid$，输出1，更新下界
   - 如果目标数 $n$ 小于 $mid$，输出0，更新上界
   - 重复6次得到6位二进制编码

---

## 代码

```cpp []
#include <iostream>
#include <vector>
using namespace std;

int main() {
    int n;
    while (cin >> n) {
        int high = 90, low = -90;
        vector<int> result;
        
        // 进行6次二分
        for (int i = 0; i < 6; ++i) {
            int mid = (low + high) / 2;
            if (n >= mid) {
                result.push_back(1);
                low = mid;
            } else {
                result.push_back(0);
                high = mid;
            }
        }
        
        // 输出二进制编码
        for (int bit : result) {
            cout << bit;
        }
        cout << endl;
    }
    return 0;
}
```

```java []
import java.util.Scanner;
import java.util.ArrayList;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {
            int n = sc.nextInt();
            int high = 90, low = -90;
            ArrayList<Integer> result = new ArrayList<>();
            
            // 进行6次二分
            for (int i = 0; i < 6; i++) {
                int mid = (low + high) / 2;
                if (n >= mid) {
                    result.add(1);
                    low = mid;
                } else {
                    result.add(0);
                    high = mid;
                }
            }
            
            // 输出二进制编码
            for (int bit : result) {
                System.out.print(bit);
            }
            System.out.println();
        }
    }
}
```

```python []
while True:
    try:
        n = int(input())
        high, low = 90, -90
        result = []
        
        # 进行6次二分
        for _ in range(6):
            mid = int((low + high) / 2)
            if n >= mid:
                result.append(1)
                low = mid
            else:
                result.append(0)
                high = mid
        
        # 输出二进制编码
        print(''.join(map(str, result)))
        
    except EOFError:
        break
```

---

## 算法及复杂度
- 算法：二分查找
- 时间复杂度：$\mathcal{O}(1)$ - 固定进行6次二分操作
- 空间复杂度：$\mathcal{O}(1)$ - 只需要存储6位二进制编码