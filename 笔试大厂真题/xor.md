## 题目
[题目链接](https://www.nowcoder.com/practice/7cffea0c097c4337821ab3ba25447c1c?tpId=182&tqId=225598&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

这是一道关于XOR（异或）运算的题目，主要思路如下：

1. 问题分析：
   - 给定 $n$ 个数字，需要将它们划分成不重叠的区间
   - 每个区间内所有数字的XOR和必须为0
   - 求最多可以划分多少个这样的区间

2. 解决方案：
   - 使用前缀XOR和的思想
   - 维护一个哈希表记录已经出现过的XOR和
   - 当遇到重复的XOR和时，说明找到了一个有效区间
   - 清空哈希表重新开始统计

3. 实现细节：
   - 初始时哈希表中放入0
   - 使用变量 $res$ 维护当前的XOR和
   - 使用变量 $ans$ 记录找到的区间数

---

## 代码

```cpp []
#include <iostream>
#include <unordered_map>
using namespace std;

int main() {
    int n, input;
    cin >> n;
    
    unordered_map<int, bool> mp;
    int res = 0, ans = 0;
    mp[0] = true;  // 初始化，放入0
    
    for (int i = 0; i < n; i++) {
        cin >> input;
        res ^= input;  // 计算当前的XOR和
        
        if (mp[res]) {  // 如果当前XOR和已经出现过
            mp.clear();  // 清空哈希表
            ans++;      // 找到一个新区间
            mp[0] = true;  // 重新初始化
            res = 0;
        }
        mp[res] = true;  // 记录当前XOR和
    }
    
    cout << ans << endl;
    return 0;
}
```

```java []
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        
        HashMap<Integer, Boolean> mp = new HashMap<>();
        int res = 0, ans = 0;
        mp.put(0, true);  // 初始化，放入0
        
        for (int i = 0; i < n; i++) {
            int input = sc.nextInt();
            res ^= input;  // 计算当前的XOR和
            
            if (mp.containsKey(res)) {  // 如果当前XOR和已经出现过
                mp.clear();  // 清空哈希表
                ans++;      // 找到一个新区间
                mp.put(0, true);  // 重新初始化
                res = 0;
            }
            mp.put(res, true);  // 记录当前XOR和
        }
        
        System.out.println(ans);
        sc.close();
    }
}
```

```python []
def main():
    n = int(input())
    nums = list(map(int, input().split()))
    
    mp = {0: True}  # 初始化，放入0
    res = 0
    ans = 0
    
    for num in nums:
        res ^= num  # 计算当前的XOR和
        
        if res in mp:  # 如果当前XOR和已经出现过
            mp.clear()  # 清空哈希表
            ans += 1    # 找到一个新区间
            mp[0] = True  # 重新初始化
            res = 0
        mp[res] = True  # 记录当前XOR和
    
    print(ans)

if __name__ == "__main__":
    main()
```

---

## 算法及复杂度
- 算法：前缀XOR + 哈希表
- 时间复杂度：$\mathcal{O}(n)$ - 只需要遍历一次数组
- 空间复杂度：$\mathcal{O}(n)$ - 哈希表最大可能存储 $n$ 个不同的XOR和