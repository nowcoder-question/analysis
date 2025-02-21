## 题目
[题目链接](https://www.nowcoder.com/practice/f0f1015579904ebc92974f7c92764797?tpId=182&tqId=325921&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

这是一道哈希表应用题，主要思路如下：

1. 问题分析：
   - 需要实现IP白名单的增删查功能
   - 输入格式为"type:ip"
   - type包括：i(插入)、d(删除)、s(查找)
   - 需要高效处理大量IP地址

2. 解决方案：
   - 使用哈希表存储IP地址
   - 根据命令类型执行相应操作
   - 使用unordered_map实现O(1)的查找效率
   - 处理每行输入直到遇到"end"

3. 实现细节：
   - 解析输入字符串获取命令和IP
   - 根据命令执行相应操作
   - 输出操作结果

---

## 代码

```cpp []
#include <iostream>
#include <string>
#include <unordered_map>
using namespace std;

int main() {
    string s;
    unordered_map<string, int> dict;
    
    while (getline(cin, s)) {
        if (s == "end") break;
        
        // 解析命令和IP
        char command = s[0];
        string ip = s.substr(2);  // 跳过type和冒号
        
        // 根据命令执行操作
        switch(command) {
            case 'i':  // 插入
                dict[ip] = 1;
                cout << "ok" << endl;
                break;
            case 'd':  // 删除
                dict.erase(ip);
                cout << "ok" << endl;
                break;
            case 's':  // 查找
                cout << (dict.count(ip) > 0 ? "true" : "false") << endl;
                break;
        }
    }
    return 0;
}
```

```java []
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        HashMap<String, Integer> dict = new HashMap<>();
        
        while (sc.hasNextLine()) {
            String s = sc.nextLine();
            if (s.equals("end")) break;
            
            // 解析命令和IP
            char command = s.charAt(0);
            String ip = s.substring(2);  // 跳过type和冒号
            
            // 根据命令执行操作
            switch(command) {
                case 'i':  // 插入
                    dict.put(ip, 1);
                    System.out.println("ok");
                    break;
                case 'd':  // 删除
                    dict.remove(ip);
                    System.out.println("ok");
                    break;
                case 's':  // 查找
                    System.out.println(dict.containsKey(ip) ? "true" : "false");
                    break;
            }
        }
        sc.close();
    }
}
```

```python []
def process_ip_whitelist():
    dict = {}
    
    while True:
        try:
            s = input()
            if s == "end":
                break
            
            # 解析命令和IP
            command = s[0]
            ip = s[2:]  # 跳过type和冒号
            
            # 根据命令执行操作
            if command == 'i':  # 插入
                dict[ip] = 1
                print("ok")
            elif command == 'd':  # 删除
                if ip in dict:
                    del dict[ip]
                print("ok")
            elif command == 's':  # 查找
                print("true" if ip in dict else "false")
                
        except EOFError:
            break

if __name__ == "__main__":
    process_ip_whitelist()
```

---

## 算法及复杂度
- 算法：哈希表
- 时间复杂度：$\mathcal{O}(1)$ - 每个操作的平均时间复杂度
- 空间复杂度：$\mathcal{O}(n)$ - $n$ 为存储的IP地址数量
