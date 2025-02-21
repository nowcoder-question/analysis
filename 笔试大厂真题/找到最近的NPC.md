## 题目
[题目链接](https://www.nowcoder.com/practice/738a3511d8d14f189481a62e725e2775?tpId=182&tqId=371116&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

1. 解析输入数据，获取玩家坐标和所有 $NPC$ 坐标
2. 计算玩家到每个 $NPC$ 的欧几里得距离
3. 找到最短距离对应的 $NPC$ 坐标
4. 按要求格式输出结果

---

## 代码

```cpp []
#include <iostream>
#include <vector>
#include <string>
#include <sstream>
#include <cmath>
using namespace std;

int main() {
    // 读取玩家坐标和NPC数量
    int playerX, playerY, n;
    char comma;
    cin >> playerX >> comma >> playerY >> comma >> n >> comma;
    
    // 读取NPC坐标
    vector<pair<int, int>> npcs;
    string input;
    cin >> input;
    stringstream ss(input);
    for(int i = 0; i < n; i++) {
        int x, y;
        ss >> x >> comma >> y;
        if(i < n-1) ss >> comma;
        npcs.push_back({x, y});
    }
    
    // 找最近的NPC
    double minDist = 1e9;
    int resX = 0, resY = 0;
    
    for(auto& npc : npcs) {
        double dist = sqrt(pow(playerX - npc.first, 2) + 
                         pow(playerY - npc.second, 2));
        if(dist < minDist) {
            minDist = dist;
            resX = npc.first;
            resY = npc.second;
        }
    }
    
    // 输出结果
    cout << "(" << resX << "," << resY << ")" << endl;
    
    return 0;
}
```
```java []
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        // 去除所有空格后再按逗号分割
        String[] input = sc.nextLine().replaceAll("\\s+", "").split(",");
        
        try {
            // 解析玩家坐标和NPC数量
            int playerX = Integer.parseInt(input[0]);
            int playerY = Integer.parseInt(input[1]);
            int n = Integer.parseInt(input[2]);
            
            // 找最近的NPC
            double minDist = Double.MAX_VALUE;
            int resX = 0, resY = 0;
            
            // 遍历所有NPC
            for(int i = 0; i < n; i++) {
                int npcX = Integer.parseInt(input[3 + i*2]);
                int npcY = Integer.parseInt(input[4 + i*2]);
                
                // 确保坐标在地图范围内
                if(npcX >= 0 && npcX < 128 && npcY >= 0 && npcY < 128) {
                    double dist = Math.sqrt(Math.pow(playerX - npcX, 2) + 
                                         Math.pow(playerY - npcY, 2));
                    
                    if(dist < minDist) {
                        minDist = dist;
                        resX = npcX;
                        resY = npcY;
                    }
                }
            }
            
            // 输出结果
            System.out.printf("(%d,%d)\n", resX, resY);
            
        } catch(Exception e) {
            // 处理可能的数组越界或数字格式异常
            System.out.println("输入格式错误");
        }
    }
}
```
```python []
def solve():
    # 读取输入
    inputs = input().split(',')
    playerX = int(inputs[0])
    playerY = int(inputs[1])
    n = int(inputs[2])
    
    # 解析NPC坐标
    npcs = []
    for i in range(n):
        x = int(inputs[3 + i*2])
        y = int(inputs[4 + i*2])
        npcs.append((x, y))
    
    # 找最近的NPC
    min_dist = float('inf')
    res_x = res_y = 0
    
    for x, y in npcs:
        dist = ((playerX - x) ** 2 + (playerY - y) ** 2) ** 0.5
        if dist < min_dist:
            min_dist = dist
            res_x, res_y = x, y
    
    # 输出结果
    print(f"({res_x},{res_y})")

if __name__ == "__main__":
    solve()
```

---

## 算法及复杂度
- 算法：遍历计算欧几里得距离
- 时间复杂度：$\mathcal{O}(n)$，$n$ 为 $NPC$ 数量
- 空间复杂度：$\mathcal{O}(n)$，存储 $NPC$ 坐标
