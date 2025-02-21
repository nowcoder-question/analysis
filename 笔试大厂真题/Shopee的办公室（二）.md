## 题目
[题目链接](https://www.nowcoder.com/practice/a71f3bd890734201986cd1e171807d30?tpId=182&tqId=377286&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

这是一道典型的网格路径动态规划问题：
1. 从左上角 $(0,0)$ 出发，每次只能向右或向上移动
2. 需要避开boss所在的位置
3. 求到达右上角 $(x,y)$ 的所有可能路径数

动态规划思路：
1. 创建 $dp$ 数组，$dp[i][j]$ 表示到达位置 $(i,j)$ 的路径数
2. 如果当前位置有boss，则 $dp[i][j] = 0$
3. 否则 $dp[i][j] = dp[i-1][j] + dp[i][j-1]$
4. 初始条件：$dp[0][0] = 1$

---

## 代码

```cpp
#include <iostream>
using namespace std;

int main() {
    int x, y, n;
    cin >> x >> y >> n;
    
    // 初始化网格，标记boss位置
    long long grid[31][31] = {0};
    bool boss[31][31] = {false};
    for(int i = 0; i < n; i++) {
        int bx, by;
        cin >> bx >> by;
        boss[bx][by] = true;
    }
    
    // 动态规划计算路径数
    grid[0][0] = 1;
    for(int i = 0; i <= x; i++) {
        for(int j = 0; j <= y; j++) {
            if(boss[i][j]) {
                grid[i][j] = 0;
            } else {
                if(i > 0) grid[i][j] += grid[i-1][j];
                if(j > 0) grid[i][j] += grid[i][j-1];
            }
        }
    }
    
    cout << grid[x][y] << endl;
    return 0;
}
```

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int x = sc.nextInt();
        int y = sc.nextInt();
        int n = sc.nextInt();
        
        // 初始化网格和boss位置
        long[][] grid = new long[31][31];
        boolean[][] boss = new boolean[31][31];
        
        for(int i = 0; i < n; i++) {
            int bx = sc.nextInt();
            int by = sc.nextInt();
            boss[bx][by] = true;
        }
        
        // 动态规划
        grid[0][0] = 1;
        for(int i = 0; i <= x; i++) {
            for(int j = 0; j <= y; j++) {
                if(boss[i][j]) {
                    grid[i][j] = 0;
                } else {
                    if(i > 0) grid[i][j] += grid[i-1][j];
                    if(j > 0) grid[i][j] += grid[i][j-1];
                }
            }
        }
        
        System.out.println(grid[x][y]);
    }
}
```

```python
x, y, n = map(int, input().split())

# 初始化网格和boss位置
grid = [[0] * 31 for _ in range(31)]
boss = [[False] * 31 for _ in range(31)]

# 标记boss位置
for _ in range(n):
    bx, by = map(int, input().split())
    boss[bx][by] = True

# 动态规划
grid[0][0] = 1
for i in range(x + 1):
    for j in range(y + 1):
        if boss[i][j]:
            grid[i][j] = 0
        else:
            if i > 0:
                grid[i][j] += grid[i-1][j]
            if j > 0:
                grid[i][j] += grid[i][j-1]

print(grid[x][y])
```

---

## 算法及复杂度
- 算法：动态规划
- 时间复杂度：$\mathcal{O}(xy)$ - 需要遍历整个网格
- 空间复杂度：$\mathcal{O}(xy)$ - 需要存储 $dp$ 数组和boss位置数组