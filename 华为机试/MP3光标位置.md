## 题目
[题目链接](https://www.nowcoder.com/practice/eaf5b886bd6645dd9cfb5406f3753e15?tpId=37&tqId=36888&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

这是一个模拟题，需要处理以下几种情况：
1. 歌曲总数 ≤ 4：不需要翻页，只移动光标
   - Up键：光标在第一首时移到最后，否则向上移动
   - Down键：光标在最后时移到第一首，否则向下移动

2. 歌曲总数 > 4：需要处理翻页
   - 特殊翻页：
     - 第一页按Up：显示最后一页，光标到最后
     - 最后一页按Down：显示第一页，光标到最前
   - 一般翻页：
     - 光标在当前页第一首按Up：向上翻页
     - 光标在当前页最后一首按Down：向下翻页

## 代码

``` cpp []
#include <iostream>
#include <vector>
using namespace std;

class MP3Player {
private:
    int n;          // 总歌曲数
    int cursor;     // 当前光标位置（1-based）
    int pageStart;  // 当前页面起始歌曲编号（1-based）
    
    void handleSmallList(char cmd) {
        if (cmd == 'U') {
            cursor = (cursor == 1) ? n : cursor - 1;
        } else {
            cursor = (cursor == n) ? 1 : cursor + 1;
        }
    }
    
    void handleLargeList(char cmd) {
        if (cmd == 'U') {
            if (cursor == 1) {
                // 特殊翻页：第一首歌按Up
                cursor = n;
                pageStart = max(1, n - 3);
            } else {
                cursor--;
                if (cursor < pageStart) {
                    pageStart = cursor;
                }
            }
        } else {
            if (cursor == n) {
                // 特殊翻页：最后一首歌按Down
                cursor = 1;
                pageStart = 1;
            } else {
                cursor++;
                if (cursor > pageStart + 3) {
                    pageStart = cursor - 3;
                }
            }
        }
    }
    
public:
    MP3Player(int totalSongs) : n(totalSongs), cursor(1), pageStart(1) {}
    
    void process(char cmd) {
        if (n <= 4) {
            handleSmallList(cmd);
        } else {
            handleLargeList(cmd);
        }
    }
    
    void display() {
        // 显示当前页面
        int end = min(pageStart + 3, n);
        for (int i = pageStart; i <= end; i++) {
            cout << i << " ";
        }
        cout << endl;
        // 显示当前选中歌曲
        cout << cursor << endl;
    }
};

int main() {
    int n;
    string commands;
    cin >> n >> commands;
    
    MP3Player player(n);
    for (char cmd : commands) {
        player.process(cmd);
    }
    player.display();
    
    return 0;
}
```
``` java []
import java.util.*;

public class Main {
    static class MP3Player {
        private int n;          // 总歌曲数
        private int cursor;     // 当前光标位置（1-based）
        private int pageStart;  // 当前页面起始歌曲编号（1-based）
        
        public MP3Player(int totalSongs) {
            this.n = totalSongs;
            this.cursor = 1;
            this.pageStart = 1;
        }
        
        private void handleSmallList(char cmd) {
            if (cmd == 'U') {
                cursor = (cursor == 1) ? n : cursor - 1;
            } else {
                cursor = (cursor == n) ? 1 : cursor + 1;
            }
        }
        
        private void handleLargeList(char cmd) {
            if (cmd == 'U') {
                if (cursor == 1) {
                    cursor = n;
                    pageStart = Math.max(1, n - 3);
                } else {
                    cursor--;
                    if (cursor < pageStart) {
                        pageStart = cursor;
                    }
                }
            } else {
                if (cursor == n) {
                    cursor = 1;
                    pageStart = 1;
                } else {
                    cursor++;
                    if (cursor > pageStart + 3) {
                        pageStart = cursor - 3;
                    }
                }
            }
        }
        
        public void process(char cmd) {
            if (n <= 4) {
                handleSmallList(cmd);
            } else {
                handleLargeList(cmd);
            }
        }
        
        public void display() {
            int end = Math.min(pageStart + 3, n);
            for (int i = pageStart; i <= end; i++) {
                System.out.print(i + " ");
            }
            System.out.println();
            System.out.println(cursor);
        }
    }
    
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        String commands = sc.next();
        
        MP3Player player = new MP3Player(n);
        for (char cmd : commands.toCharArray()) {
            player.process(cmd);
        }
        player.display();
    }
}
```
``` python []
class MP3Player:
    def __init__(self, total_songs):
        self.n = total_songs        # 总歌曲数
        self.cursor = 1             # 当前光标位置（1-based）
        self.page_start = 1         # 当前页面起始歌曲编号（1-based）
    
    def handle_small_list(self, cmd):
        if cmd == 'U':
            self.cursor = self.n if self.cursor == 1 else self.cursor - 1
        else:
            self.cursor = 1 if self.cursor == self.n else self.cursor + 1
    
    def handle_large_list(self, cmd):
        if cmd == 'U':
            if self.cursor == 1:
                # 特殊翻页：第一首歌按Up
                self.cursor = self.n
                self.page_start = max(1, self.n - 3)
            else:
                self.cursor -= 1
                if self.cursor < self.page_start:
                    self.page_start = self.cursor
        else:
            if self.cursor == self.n:
                # 特殊翻页：最后一首歌按Down
                self.cursor = 1
                self.page_start = 1
            else:
                self.cursor += 1
                if self.cursor > self.page_start + 3:
                    self.page_start = self.cursor - 3
    
    def process(self, cmd):
        if self.n <= 4:
            self.handle_small_list(cmd)
        else:
            self.handle_large_list(cmd)
    
    def display(self):
        # 显示当前页面
        end = min(self.page_start + 3, self.n)
        print(' '.join(map(str, range(self.page_start, end + 1))))
        # 显示当前选中歌曲
        print(self.cursor)

# 处理输入
n = int(input())
commands = input().strip()

player = MP3Player(n)
for cmd in commands:
    player.process(cmd)
player.display()
```

---

## 算法及复杂度
- 算法：模拟
- 时间复杂度：$\mathcal{O}(m)$，其中 $m$ 是命令的长度
- 空间复杂度：$\mathcal{O}(1)$，只需要常数额外空间

这个解法通过模拟MP3播放器的行为，维护当前光标位置和页面起始位置，根据不同情况处理上下键的操作。代码结构清晰，分别处理了歌曲数量小于等于4和大于4的情况。
