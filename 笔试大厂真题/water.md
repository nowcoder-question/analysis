## 题目
[题目链接](https://www.nowcoder.com/practice/9e988ccf4d324b6d8afe4b4b172968ee?tpId=182&tqId=226089&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

这是一道经典的倒水问题，需要通过BFS搜索来找到从初始状态到目标状态的最短路径。主要思路如下：

1. 使用四维布尔数组 $mem[64][64][64][64]$ 记录状态是否访问过
2. 使用 `tuple<int,int,int,int>` 表示四个杯子的状态，简化状态传递
3. 使用BFS搜索所有可能的状态转移，每次可以:
   - 将任意杯子装满水
   - 将任意杯子倒空
   - 将一个杯子的水倒入另一个杯子
4. 特殊情况处理：如果初始容量全为0且不等于目标状态，直接返回-1

---

## 代码

```cpp []
#include<bits/stdc++.h>
using namespace std;

bool mem[64][64][64][64] = {false};

bool equal(int s[4], int T[4]){
    return (s[0]==T[0])&&(s[1]==T[1])&&(s[2]==T[2])&&(s[3]==T[3]);
}

int bfs(int S[4], int T[4]){
    queue<tuple<int,int,int,int>> q;
    auto x = make_tuple(0,0,0,0);
    q.push(x);
    mem[0][0][0][0] = true;
    int step = 0, cap[4]={0};
    
    while(!q.empty()){
        int size = q.size();
        while(size--){
            tie(cap[0],cap[1],cap[2],cap[3]) = q.front(); q.pop();
            if(equal(cap, T)){
                return step;
            }
            
            for(int i=0; i<3; ++i){
                switch(i){
                    case 0: // 将某个杯子倒满
                        for(int j=0, tmp=0; j<4; ++j){
                            tmp = cap[j];
                            cap[j] = S[j];
                            x = make_tuple(cap[0], cap[1], cap[2], cap[3]);
                            if(!mem[cap[0]][cap[1]][cap[2]][cap[3]]){
                                mem[cap[0]][cap[1]][cap[2]][cap[3]]=true;
                                q.push(x);
                            }
                            cap[j] = tmp;
                        }
                        break;
                    case 1: // 将某个杯子倒空
                        for(int j=0, tmp=0; j<4; ++j){
                            tmp = cap[j];
                            cap[j] = 0;
                            x = make_tuple(cap[0], cap[1], cap[2], cap[3]);
                            if(!mem[cap[0]][cap[1]][cap[2]][cap[3]]){
                                mem[cap[0]][cap[1]][cap[2]][cap[3]]=true;
                                q.push(x);
                            }
                            cap[j] = tmp;
                        }
                        break;
                    case 2: // 将杯子j倒向杯子z
                        for(int j=0; j<4; ++j){
                            int tmp1,tmp2,need;
                            for(int z=0; z<4; ++z){
                                if(j==z) continue;
                                tmp1=cap[j], tmp2=cap[z];
                                need = min(cap[j], S[z]-cap[z]);
                                cap[j] -= need;
                                cap[z] += need;
                                x = make_tuple(cap[0], cap[1], cap[2], cap[3]);
                                if(!mem[cap[0]][cap[1]][cap[2]][cap[3]]){
                                    mem[cap[0]][cap[1]][cap[2]][cap[3]]=true;
                                    q.push(x);
                                }
                                cap[j]=tmp1, cap[z]=tmp2;
                            }
                        }
                }
            }
        }
        ++step;
    }
    return -1;
}

int main(){
    int S[4]={0}, T[4]={0};
    cin>> S[0] >> S[1] >> S[2] >> S[3];
    cin>> T[0] >> T[1] >> T[2] >> T[3];
    
    // 特殊情况处理
    if(S[0]==0 && S[1]==0 && S[2]==0 && S[3]==0 && !equal(S,T)){
        cout<< -1 << endl;
    }else{
        int ret = bfs(S, T);
        cout<< ret << endl;
    }
    return 0;
}
```

```java []
import java.util.*;

public class Main {
    static boolean[][][][] mem = new boolean[64][64][64][64];
    
    static boolean equal(int[] s, int[] t) {
        return s[0] == t[0] && s[1] == t[1] && s[2] == t[2] && s[3] == t[3];
    }
    
    static int bfs(int[] S, int[] T) {
        Queue<int[]> q = new LinkedList<>();
        q.offer(new int[]{0,0,0,0});
        mem[0][0][0][0] = true;
        int step = 0;
        
        while(!q.isEmpty()) {
            int size = q.size();
            while(size-- > 0) {
                int[] cap = q.poll();
                if(equal(cap, T)) {
                    return step;
                }
                
                for(int i = 0; i < 3; i++) {
                    switch(i) {
                        case 0: // 将某个杯子倒满
                            for(int j = 0; j < 4; j++) {
                                int tmp = cap[j];
                                cap[j] = S[j];
                                if(!mem[cap[0]][cap[1]][cap[2]][cap[3]]) {
                                    mem[cap[0]][cap[1]][cap[2]][cap[3]] = true;
                                    q.offer(cap.clone());
                                }
                                cap[j] = tmp;
                            }
                            break;
                        case 1: // 将某个杯子倒空
                            for(int j = 0; j < 4; j++) {
                                int tmp = cap[j];
                                cap[j] = 0;
                                if(!mem[cap[0]][cap[1]][cap[2]][cap[3]]) {
                                    mem[cap[0]][cap[1]][cap[2]][cap[3]] = true;
                                    q.offer(cap.clone());
                                }
                                cap[j] = tmp;
                            }
                            break;
                        case 2: // 将杯子j倒向杯子z
                            for(int j = 0; j < 4; j++) {
                                for(int z = 0; z < 4; z++) {
                                    if(j == z) continue;
                                    int tmp1 = cap[j], tmp2 = cap[z];
                                    int need = Math.min(cap[j], S[z]-cap[z]);
                                    cap[j] -= need;
                                    cap[z] += need;
                                    if(!mem[cap[0]][cap[1]][cap[2]][cap[3]]) {
                                        mem[cap[0]][cap[1]][cap[2]][cap[3]] = true;
                                        q.offer(cap.clone());
                                    }
                                    cap[j] = tmp1;
                                    cap[z] = tmp2;
                                }
                            }
                    }
                }
            }
            step++;
        }
        return -1;
    }
    
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int[] S = new int[4];
        int[] T = new int[4];
        
        for(int i = 0; i < 4; i++) S[i] = sc.nextInt();
        for(int i = 0; i < 4; i++) T[i] = sc.nextInt();
        
        if(S[0] == 0 && S[1] == 0 && S[2] == 0 && S[3] == 0 && !equal(S,T)) {
            System.out.println(-1);
        } else {
            System.out.println(bfs(S, T));
        }
    }
}
```

```python []
from collections import deque

def equal(s, t):
    return s[0] == t[0] and s[1] == t[1] and s[2] == t[2] and s[3] == t[3]

def bfs(S, T):
    mem = [[[[ False for _ in range(64)] for _ in range(64)] for _ in range(64)] for _ in range(64)]
    q = deque([(0,0,0,0)])
    mem[0][0][0][0] = True
    step = 0
    
    while q:
        size = len(q)
        for _ in range(size):
            cap = list(q.popleft())
            if equal(cap, T):
                return step
                
            for i in range(3):
                if i == 0:  # 将某个杯子倒满
                    for j in range(4):
                        tmp = cap[j]
                        cap[j] = S[j]
                        if not mem[cap[0]][cap[1]][cap[2]][cap[3]]:
                            mem[cap[0]][cap[1]][cap[2]][cap[3]] = True
                            q.append(tuple(cap))
                        cap[j] = tmp
                        
                elif i == 1:  # 将某个杯子倒空
                    for j in range(4):
                        tmp = cap[j]
                        cap[j] = 0
                        if not mem[cap[0]][cap[1]][cap[2]][cap[3]]:
                            mem[cap[0]][cap[1]][cap[2]][cap[3]] = True
                            q.append(tuple(cap))
                        cap[j] = tmp
                        
                else:  # 将杯子j倒向杯子z
                    for j in range(4):
                        for z in range(4):
                            if j == z:
                                continue
                            tmp1, tmp2 = cap[j], cap[z]
                            need = min(cap[j], S[z]-cap[z])
                            cap[j] -= need
                            cap[z] += need
                            if not mem[cap[0]][cap[1]][cap[2]][cap[3]]:
                                mem[cap[0]][cap[1]][cap[2]][cap[3]] = True
                                q.append(tuple(cap))
                            cap[j], cap[z] = tmp1, tmp2
        step += 1
    return -1

if __name__ == "__main__":
    S = list(map(int, input().split()))
    T = list(map(int, input().split()))
    
    if S == [0,0,0,0] and not equal(S,T):
        print(-1)
    else:
        print(bfs(S, T))
```

---

## 算法及复杂度分析

### 优化要点：
1. 使用四维布尔数组替代哈希表，大幅提升访问效率
2. 使用tuple简化状态表示和传递
3. 使用switch-case结构清晰地划分三种操作
4. 提前处理特殊情况（全0初始状态）

### 复杂度分析：
- 时间复杂度：$\mathcal{O}(V+E)$，其中 $V$ 是状态数（最大 $64^4$），$E$ 是状态转移数
- 空间复杂度：$\mathcal{O}(64^4)$，使用固定大小的四维数组
