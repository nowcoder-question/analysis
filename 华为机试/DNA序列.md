## 题目
[题目链接](https://www.nowcoder.com/practice/e8480ed7501640709354db1cc4ffd42a?tpId=37&tqId=36887&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

这道题要求在DNA序列中找出GC-Ratio最高的子串，需要注意以下几点：
1. GC-Ratio = (G和C的个数) / 子串长度
2. 子串长度固定为输入的N
3. 如果有多个子串的GC-Ratio相同，输出第一个

解题步骤：
1. 使用滑动窗口遍历所有长度为N的子串
2. 对每个子串计算GC-Ratio
3. 记录最大GC-Ratio及其对应的子串
4. 如果遇到相同的GC-Ratio，保持第一个找到的子串不变

---

## 代码

```c++
#include <iostream>
#include <string>
using namespace std;

string findMaxGCRatio(string dna, int n) {
    if (dna.length() < n) return "";
    
    // 计算第一个窗口的GC数量
    int maxGC = 0;
    int currentGC = 0;
    for (int i = 0; i < n; i++) {
        if (dna[i] == 'G' || dna[i] == 'C') {
            currentGC++;
        }
    }
    maxGC = currentGC;
    int maxStart = 0;
    
    // 滑动窗口
    for (int i = 1; i <= dna.length() - n; i++) {
        // 移除窗口左边的字符的贡献
        if (dna[i-1] == 'G' || dna[i-1] == 'C') {
            currentGC--;
        }
        // 添加窗口右边新字符的贡献
        if (dna[i+n-1] == 'G' || dna[i+n-1] == 'C') {
            currentGC++;
        }
        // 更新最大值
        if (currentGC > maxGC) {
            maxGC = currentGC;
            maxStart = i;
        }
    }
    
    return dna.substr(maxStart, n);
}

int main() {
    string dna;
    int n;
    while (cin >> dna >> n) {
        cout << findMaxGCRatio(dna, n) << endl;
    }
    return 0;
}
```

```java
import java.util.Scanner;

public class Main {
    public static String findMaxGCRatio(String dna, int n) {
        if (dna.length() < n) return "";
        
        // 计算第一个窗口的GC数量
        int maxGC = 0;
        int currentGC = 0;
        for (int i = 0; i < n; i++) {
            if (dna.charAt(i) == 'G' || dna.charAt(i) == 'C') {
                currentGC++;
            }
        }
        maxGC = currentGC;
        int maxStart = 0;
        
        // 滑动窗口
        for (int i = 1; i <= dna.length() - n; i++) {
            if (dna.charAt(i-1) == 'G' || dna.charAt(i-1) == 'C') {
                currentGC--;
            }
            if (dna.charAt(i+n-1) == 'G' || dna.charAt(i+n-1) == 'C') {
                currentGC++;
            }
            if (currentGC > maxGC) {
                maxGC = currentGC;
                maxStart = i;
            }
        }
        
        return dna.substring(maxStart, maxStart + n);
    }
    
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {
            String dna = sc.next();
            int n = sc.nextInt();
            System.out.println(findMaxGCRatio(dna, n));
        }
    }
}
```

```python
def find_max_gc_ratio(dna, n):
    if len(dna) < n:
        return ""
    
    # 计算第一个窗口的GC数量
    current_gc = sum(1 for c in dna[:n] if c in 'GC')
    max_gc = current_gc
    max_start = 0
    
    # 滑动窗口
    for i in range(1, len(dna) - n + 1):
        # 更新当前窗口的GC数量
        if dna[i-1] in 'GC':
            current_gc -= 1
        if dna[i+n-1] in 'GC':
            current_gc += 1
            
        # 更新最大值
        if current_gc > max_gc:
            max_gc = current_gc
            max_start = i
            
    return dna[max_start:max_start+n]

while True:
    try:
        dna = input().strip()
        n = int(input())
        print(find_max_gc_ratio(dna, n))
    except:
        break
```

---

## 算法及复杂度
- 算法：滑动窗口
- 时间复杂度：$\mathcal{O}(N)$，其中 $N$ 是DNA序列的长度
- 空间复杂度：$\mathcal{O}(1)$，只需要常数额外空间
