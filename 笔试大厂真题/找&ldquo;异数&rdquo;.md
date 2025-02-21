## 题目
[题目链接](https://www.nowcoder.com/practice/596eb514664b415994c843c2e489aba8?tpId=182&tqId=325931&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

题目要求找出序列中的"异数"，定义如下：
1. 序列中包含 $2$ 到 $16$ 进制的整数
2. 如果一个数与序列中其他所有数都不相等，则称为"异数"
3. 输入格式为"n#m"，其中 $n$ 表示进制 $(1<n<17)$，$m$ 为该进制下的数值

解题思路：
1. 对每个输入的数：
   - 解析进制 $n$ 和数值 $m$
   - 将 $m$ 转换为十进制数值
2. 使用哈希表统计每个十进制值出现的次数
3. 输出只出现一次的数的原始表示形式

---

## 代码

```cpp []
#include <iostream>
#include <string>
#include <vector>
#include <unordered_map>
using namespace std;

// 将指定进制的字符串转换为十进制数
int convertToDecimal(int base, const string& num) {
    int result = 0;
    for (char c : num) {
        int digit;
        if (c >= '0' && c <= '9') {
            digit = c - '0';
        } else {
            digit = c - 'A' + 10;
        }
        result = result * base + digit;
    }
    return result;
}

int main() {
    string input;
    vector<pair<string, int>> numbers;
    unordered_map<int, int> frequency;
    
    // 读取输入直到遇到END
    while (cin >> input && input != "END") {
        // 分割进制和数值
        int pos = input.find('#');
        int base = stoi(input.substr(0, pos));
        string num = input.substr(pos + 1);
        
        // 转换为十进制并存储
        int decimal = convertToDecimal(base, num);
        numbers.push_back({input, decimal});
        frequency[decimal]++;
    }
    
    // 输出异数
    bool found = false;
    for (const auto& p : numbers) {
        if (frequency[p.second] == 1) {
            cout << p.first << endl;
            found = true;
        }
    }
    
    if (!found) {
        cout << "None" << endl;
    }
    
    return 0;
}
```
```java []
import java.util.*;

public class Main {
    // 将指定进制的字符串转换为十进制数
    private static int convertToDecimal(int base, String num) {
        int result = 0;
        for (char c : num.toCharArray()) {
            int digit;
            if (c >= '0' && c <= '9') {
                digit = c - '0';
            } else {
                digit = c - 'A' + 10;
            }
            result = result * base + digit;
        }
        return result;
    }
    
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        List<String> originalInputs = new ArrayList<>();
        List<Integer> decimalValues = new ArrayList<>();
        Map<Integer, Integer> frequency = new HashMap<>();
        
        // 读取输入
        while (true) {
            String input = sc.next();
            if (input.equals("END")) break;
            
            // 分割进制和数值
            String[] parts = input.split("#");
            int base = Integer.parseInt(parts[0]);
            int decimal = convertToDecimal(base, parts[1]);
            
            originalInputs.add(input);
            decimalValues.add(decimal);
            frequency.put(decimal, frequency.getOrDefault(decimal, 0) + 1);
        }
        
        // 输出异数
        boolean found = false;
        for (int i = 0; i < originalInputs.size(); i++) {
            if (frequency.get(decimalValues.get(i)) == 1) {
                System.out.println(originalInputs.get(i));
                found = true;
            }
        }
        
        if (!found) {
            System.out.println("None");
        }
    }
}
```
```python []
def convert_to_decimal(base, num):
    result = 0
    for c in num:
        if c.isdigit():
            digit = int(c)
        else:
            digit = ord(c) - ord('A') + 10
        result = result * base + digit
    return result

# 存储输入和转换后的值
numbers = []  # 存储原始输入和十进制值的元组
frequency = {}  # 存储十进制值的出现频率

# 读取输入
while True:
    try:
        inp = input()
        if inp == "END":
            break
            
        # 分割进制和数值
        base, num = inp.split('#')
        base = int(base)
        
        # 转换为十进制
        decimal = convert_to_decimal(base, num)
        
        numbers.append((inp, decimal))
        frequency[decimal] = frequency.get(decimal, 0) + 1
        
    except EOFError:
        break

# 输出异数
found = False
for original, decimal in numbers:
    if frequency[decimal] == 1:
        print(original)
        found = True

if not found:
    print("None")
```

---

## 算法及复杂度
- 算法：进制转换 + 哈希表统计  
- 时间复杂度：$\mathcal{O}(NL)$ - $N$ 是输入数的个数，$L$ 是每个数的最大长度  
- 空间复杂度：$\mathcal{O}(N)$ - 需要存储所有输入的原始形式和十进制值