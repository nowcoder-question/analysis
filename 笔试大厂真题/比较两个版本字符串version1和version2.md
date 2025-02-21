## 题目
[题目链接](https://www.nowcoder.com/practice/521049ca23f147d698e1cff923c3262a?tpId=182&tqId=362282&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

这是一道字符串处理题目，主要思路如下：

1. 使用分隔符'.'将版本号分割成子版本号
2. 逐个比较对应位置的子版本号：
   - 如果version1的子版本号大，返回1
   - 如果version2的子版本号大，返回-1
   - 如果相等，继续比较下一组
3. 如果其中一个版本号较短：
   - version1较短，返回-1
   - version2较短，返回1
   - 长度相等且所有子版本号都相等，返回0

---

## 代码

```cpp []
#include <iostream>
#include <sstream>
#include <string>
using namespace std;

int main() {
    string version1, version2;
    cin >> version1 >> version2;
    
    // 使用字符串流分割版本号
    stringstream ss1(version1), ss2(version2);
    string seg1, seg2;
    
    // 逐个比较子版本号
    while(getline(ss1, seg1, '.') && getline(ss2, seg2, '.')) {
        int v1 = stoi(seg1);
        int v2 = stoi(seg2);
        
        if(v1 > v2) {
            cout << 1 << endl;
            return 0;
        }
        if(v1 < v2) {
            cout << -1 << endl;
            return 0;
        }
    }
    
    // 处理长度不等的情况
    if(version1.size() < version2.size()) {
        cout << -1 << endl;
    }
    else if(version1.size() > version2.size()) {
        cout << 1 << endl;
    }
    else {
        cout << 0 << endl;
    }
    
    return 0;
}
```

```java []
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String version1 = sc.next();
        String version2 = sc.next();
        
        // 分割版本号
        String[] v1 = version1.split("\\.");
        String[] v2 = version2.split("\\.");
        
        // 获取最小长度
        int len = Math.min(v1.length, v2.length);
        
        // 逐个比较子版本号
        for(int i = 0; i < len; i++) {
            int num1 = Integer.parseInt(v1[i]);
            int num2 = Integer.parseInt(v2[i]);
            
            if(num1 > num2) {
                System.out.println(1);
                return;
            }
            if(num1 < num2) {
                System.out.println(-1);
                return;
            }
        }
        
        // 处理长度不等的情况
        if(v1.length < v2.length) {
            System.out.println(-1);
        }
        else if(v1.length > v2.length) {
            System.out.println(1);
        }
        else {
            System.out.println(0);
        }
    }
}
```

```python []
def compare_versions(version1, version2):
    # 分割版本号
    v1 = version1.split('.')
    v2 = version2.split('.')
    
    # 获取最小长度
    length = min(len(v1), len(v2))
    
    # 逐个比较子版本号
    for i in range(length):
        num1 = int(v1[i])
        num2 = int(v2[i])
        
        if num1 > num2:
            return 1
        if num1 < num2:
            return -1
    
    # 处理长度不等的情况
    if len(v1) < len(v2):
        return -1
    elif len(v1) > len(v2):
        return 1
    else:
        return 0

def main():
    # 输入两个版本号
    version1, version2 = input().split()
    
    # 输出比较结果
    print(compare_versions(version1, version2))

if __name__ == "__main__":
    main()
```

---

## 算法及复杂度
- 算法：字符串分割 + 逐位比较
- 时间复杂度：$\mathcal{O}(n)$ - 其中 $n$ 为较长版本号的长度
- 空间复杂度：$\mathcal{O}(n)$ - 需要存储分割后的子版本号

## 注意事项
1. 版本号只包含数字和点号
2. 子版本号需要转换为整数进行比较
3. 需要考虑版本号长度不等的情况