## 题目
[题目链接](https://www.nowcoder.com/practice/37275e85ae7c4453920eae6b9f7f45fc?tpId=308&tqId=1714944&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

### 解题思路

1. **问题分析**
   - 需要处理带有'?'的时间格式
   - 每个'?'可以是 $0-9$ 的任意数字
   - 需要考虑时间的合法性（小时 $0-23$，分钟 $0-59$）
   - 计算两个时间之间的分钟差

2. **实现步骤**
   - 找出所有 $t1$ 和 $t2$ 可能的合法时间
   - 计算所有可能的时间差
   - 找出最大和最小时间差
---
## 代码
``` C++ []
#include <iostream>
#include <vector>
#include <set>
#include <string>
#include <climits>  // 添加此头文件以使用INT_MAX和INT_MIN
using namespace std;

// 检查时间是否合法
bool isValidTime(int hour, int minute) {
    return hour >= 0 && hour <= 23 && minute >= 0 && minute <= 59;
}

// 获取所有可能的时间（分钟表示）
set<int> getPossibleTimes(string time) {
    set<int> times;
    
    // 处理小时
    vector<int> possibleHours;
    if (time[0] == '?' && time[1] == '?') {
        for (int i = 0; i <= 23; i++) possibleHours.push_back(i);
    } else if (time[0] == '?') {
        int d2 = time[1] - '0';
        for (int i = 0; i <= 2; i++) {
            if (isValidTime(i * 10 + d2, 0)) {
                possibleHours.push_back(i * 10 + d2);
            }
        }
    } else if (time[1] == '?') {
        int d1 = time[0] - '0';
        for (int i = 0; i <= 9; i++) {
            if (isValidTime(d1 * 10 + i, 0)) {
                possibleHours.push_back(d1 * 10 + i);
            }
        }
    } else {
        int hour = (time[0] - '0') * 10 + (time[1] - '0');
        if (isValidTime(hour, 0)) possibleHours.push_back(hour);
    }
    
    // 处理分钟
    vector<int> possibleMinutes;
    if (time[3] == '?' && time[4] == '?') {
        for (int i = 0; i <= 59; i++) possibleMinutes.push_back(i);
    } else if (time[3] == '?') {
        int d2 = time[4] - '0';
        for (int i = 0; i <= 5; i++) {
            possibleMinutes.push_back(i * 10 + d2);
        }
    } else if (time[4] == '?') {
        int d1 = time[3] - '0';
        for (int i = 0; i <= 9; i++) {
            possibleMinutes.push_back(d1 * 10 + i);
        }
    } else {
        possibleMinutes.push_back((time[3] - '0') * 10 + (time[4] - '0'));
    }
    
    // 组合所有可能的时间
    for (int h : possibleHours) {
        for (int m : possibleMinutes) {
            times.insert(h * 60 + m);
        }
    }
    
    return times;
}

int main() {
    string t1, t2;
    cin >> t1 >> t2;
    
    set<int> times1 = getPossibleTimes(t1);
    set<int> times2 = getPossibleTimes(t2);
    
    int minDiff = INT_MAX;
    int maxDiff = INT_MIN;
    
    for (int time1 : times1) {
        for (int time2 : times2) {
            if (time2 > time1) {  // t1必须在t2之前
                int diff = time2 - time1;
                minDiff = min(minDiff, diff);
                maxDiff = max(maxDiff, diff);
            }
        }
    }
    
    cout << minDiff << " " << maxDiff << endl;
    return 0;
}

```


```java []
import java.util.*;

public class Main {
    // 检查时间是否合法
    static boolean isValidTime(int hour, int minute) {
        return hour >= 0 && hour <= 23 && minute >= 0 && minute <= 59;
    }
    
    // 获取所有可能的时间（分钟表示）
    static Set<Integer> getPossibleTimes(String time) {
        Set<Integer> times = new HashSet<>();
        char[] t = time.toCharArray();
        
        // 处理小时
        List<Integer> possibleHours = new ArrayList<>();
        if (t[0] == '?' && t[1] == '?') {
            for (int i = 0; i <= 23; i++) possibleHours.add(i);
        } else if (t[0] == '?') {
            int d2 = t[1] - '0';
            for (int i = 0; i <= 2; i++) {
                if (isValidTime(i * 10 + d2, 0)) {
                    possibleHours.add(i * 10 + d2);
                }
            }
        } else if (t[1] == '?') {
            int d1 = t[0] - '0';
            for (int i = 0; i <= 9; i++) {
                if (isValidTime(d1 * 10 + i, 0)) {
                    possibleHours.add(d1 * 10 + i);
                }
            }
        } else {
            int hour = (t[0] - '0') * 10 + (t[1] - '0');
            if (isValidTime(hour, 0)) possibleHours.add(hour);
        }
        
        // 处理分钟
        List<Integer> possibleMinutes = new ArrayList<>();
        if (t[3] == '?' && t[4] == '?') {
            for (int i = 0; i <= 59; i++) possibleMinutes.add(i);
        } else if (t[3] == '?') {
            int d2 = t[4] - '0';
            for (int i = 0; i <= 5; i++) {
                possibleMinutes.add(i * 10 + d2);
            }
        } else if (t[4] == '?') {
            int d1 = t[3] - '0';
            for (int i = 0; i <= 9; i++) {
                possibleMinutes.add(d1 * 10 + i);
            }
        } else {
            possibleMinutes.add((t[3] - '0') * 10 + (t[4] - '0'));
        }
        
        // 组合所有可能的时间
        for (int h : possibleHours) {
            for (int m : possibleMinutes) {
                times.add(h * 60 + m);
            }
        }
        
        return times;
    }
    
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String t1 = sc.next();
        String t2 = sc.next();
        
        Set<Integer> times1 = getPossibleTimes(t1);
        Set<Integer> times2 = getPossibleTimes(t2);
        
        int minDiff = Integer.MAX_VALUE;
        int maxDiff = Integer.MIN_VALUE;
        
        for (int time1 : times1) {
            for (int time2 : times2) {
                if (time2 > time1) {  // t1必须在t2之前
                    int diff = time2 - time1;
                    minDiff = Math.min(minDiff, diff);
                    maxDiff = Math.max(maxDiff, diff);
                }
            }
        }
        
        System.out.println(minDiff + " " + maxDiff);
    }
}
```
``` python []
def is_valid_time(hour, minute):
    return 0 <= hour <= 23 and 0 <= minute <= 59

def get_possible_times(time):
    times = set()
    
    # 处理小时
    possible_hours = []
    if time[0] == '?' and time[1] == '?':
        possible_hours = [i for i in range(24)]
    elif time[0] == '?':
        d2 = int(time[1])
        possible_hours = [i * 10 + d2 for i in range(3) if is_valid_time(i * 10 + d2, 0)]
    elif time[1] == '?':
        d1 = int(time[0])
        possible_hours = [d1 * 10 + i for i in range(10) if is_valid_time(d1 * 10 + i, 0)]
    else:
        hour = int(time[0:2])
        if is_valid_time(hour, 0):
            possible_hours = [hour]
    
    # 处理分钟
    possible_minutes = []
    if time[3] == '?' and time[4] == '?':
        possible_minutes = [i for i in range(60)]
    elif time[3] == '?':
        d2 = int(time[4])
        possible_minutes = [i * 10 + d2 for i in range(6)]
    elif time[4] == '?':
        d1 = int(time[3])
        possible_minutes = [d1 * 10 + i for i in range(10)]
    else:
        possible_minutes = [int(time[3:5])]
    
    # 组合所有可能的时间
    for h in possible_hours:
        for m in possible_minutes:
            times.add(h * 60 + m)
    
    return times

# 读入数据
t1 = input()
t2 = input()

# 获取所有可能的时间
times1 = get_possible_times(t1)
times2 = get_possible_times(t2)

# 计算最大最小时间差
min_diff = float('inf')
max_diff = float('-inf')

for time1 in times1:
    for time2 in times2:
        if time2 > time1:  # t1必须在t2之前
            diff = time2 - time1
            min_diff = min(min_diff, diff)
            max_diff = max(max_diff, diff)

print(min_diff, max_diff)
```

## 算法及复杂度分析
- **算法**：枚举
- **时间复杂度**：
   - 对于每个时间点，最多有 $24 * 60$ 种可能
   - 需要比较两个时间点的所有组合
   - $\mathcal{O}{(T1 * T2)}$，其中 $T1$ 和 $T2$ 是两个时间点的可能数量

- **空间复杂度**：$\mathcal{O}{(T1 + T2)}$，存储所有可能的时间点

## 注意事项

1. **时间合法性检查**：
   - 小时：$0-23$
   - 分钟：$0-59$

2. **边界条件**：
   - $t1$ 必须在 $t2$ 之前
   - 处理'?'的所有可能情况

3. **优化点**：
   - 使用 $\text{Set}$ 存储时间避免重复
   - 提前过滤不合法的时间
   - 转换为分钟计算更方便

