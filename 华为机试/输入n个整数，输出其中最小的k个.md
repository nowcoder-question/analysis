## 题目
[题目链接](https://www.nowcoder.com/practice/69ef2267aafd4d52b250a272fd27052c?tpId=37&tqId=36882&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

1. 题目要求：
   - 输入 $n$ 个整数，找出其中最小的 $k$ 个数
   - 按升序输出
   - 数据范围：$1 \leq n \leq 1000$，$1 \leq val \leq 10000$

2. 实现思路：
   - 读取输入的 $n$ 和 $k$
   - 读取 $n$ 个整数
   - 对数组排序
   - 输出前 $k$ 个数

3. 优化方案：
   - 可以使用快速排序
   - 也可以使用优先队列（大顶堆）
   - 本题数据范围较小，直接排序即可

---

## 代码

``` cpp []
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    int n, k;
    while (cin >> n >> k) {
        vector<int> nums(n);
        for (int i = 0; i < n; i++) {
            cin >> nums[i];
        }
        
        // 排序
        sort(nums.begin(), nums.end());
        
        // 输出前k个数
        for (int i = 0; i < k; i++) {
            cout << nums[i];
            if (i != k - 1) cout << " ";
        }
        cout << endl;
    }
    return 0;
}
```
``` java []
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {
            int n = sc.nextInt();
            int k = sc.nextInt();
            
            int[] nums = new int[n];
            for (int i = 0; i < n; i++) {
                nums[i] = sc.nextInt();
            }
            
            // 排序
            Arrays.sort(nums);
            
            // 输出前k个数
            for (int i = 0; i < k; i++) {
                System.out.print(nums[i]);
                if (i != k - 1) {
                    System.out.print(" ");
                }
            }
            System.out.println();
        }
    }
}
```
``` python []
while True:
    try:
        # 读取输入
        n, k = map(int, input().split())
        nums = list(map(int, input().split()))
        
        # 排序
        nums.sort()
        
        # 输出前k个数
        print(" ".join(map(str, nums[:k])))
        
    except EOFError:
        break
```

---

## 算法及复杂度
- 算法：排序
- 时间复杂度：$\mathcal{O}(n \log n)$ - 使用快速排序的时间复杂度
- 空间复杂度：$\mathcal{O}(n)$ - 需要存储输入的数组
