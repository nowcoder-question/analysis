## 题目
[题目链接](https://www.nowcoder.com/practice/78a1a4ebe8a34c93aac006c44f6bf8a1?tpId=37&tqId=36868&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路
数独问题是一个典型的回溯算法题目。解题思路如下：
1. 遍历整个9x9的数独板
2. 当遇到空格(值为0)时，尝试填入1-9的数字
3. 对每个尝试的数字，检查是否满足数独规则：
   - 同行不重复
   - 同列不重复
   - 同一个3x3方格内不重复
4. 如果当前数字有效，递归解决剩余的空格
5. 如果递归失败，回溯并尝试下一个数字

---

## 代码

```c++ []
#include <iostream>
#include <vector>
using namespace std;

class Solution {
public:
    bool isValid(vector<vector<int>>& board, int row, int col, int num) {
        for(int x = 0; x < 9; x++) {
            if(board[row][x] == num) return false;
        }
        
        for(int x = 0; x < 9; x++) {
            if(board[x][col] == num) return false;
        }
        
        int startRow = row - row % 3, startCol = col - col % 3;
        for(int i = 0; i < 3; i++) {
            for(int j = 0; j < 3; j++) {
                if(board[i + startRow][j + startCol] == num) return false;
            }
        }
        return true;
    }
    
    bool solveSudoku(vector<vector<int>>& board) {
        int row = -1, col = -1;
        bool isEmpty = false;
        
        for(int i = 0; i < 9; i++) {
            for(int j = 0; j < 9; j++) {
                if(board[i][j] == 0) {
                    row = i;
                    col = j;
                    isEmpty = true;
                    break;
                }
            }
            if(isEmpty) break;
        }
        
        if(!isEmpty) return true;
        
        for(int num = 1; num <= 9; num++) {
            if(isValid(board, row, col, num)) {
                board[row][col] = num;
                if(solveSudoku(board)) return true;
                board[row][col] = 0;
            }
        }
        return false;
    }
};

int main() {
    vector<vector<int>> board(9, vector<int>(9));
    
    // 读入数独
    for(int i = 0; i < 9; i++) {
        for(int j = 0; j < 9; j++) {
            cin >> board[i][j];
        }
    }
    
    Solution solution;
    solution.solveSudoku(board);
    
    // 输出结果
    for(int i = 0; i < 9; i++) {
        for(int j = 0; j < 9; j++) {
            cout << board[i][j];
            if(j < 8) cout << " ";
        }
        cout << endl;
    }
    
    return 0;
}
```

```python []
def isValid(board, row, col, num):
    for x in range(9):
        if board[row][x] == num:
            return False
            
    for x in range(9):
        if board[x][col] == num:
            return False
            
    startRow, startCol = row - row % 3, col - col % 3
    for i in range(3):
        for j in range(3):
            if board[i + startRow][j + startCol] == num:
                return False
    return True

def solveSudoku(board):
    row = col = -1
    isEmpty = False
    
    for i in range(9):
        for j in range(9):
            if board[i][j] == 0:
                row, col = i, j
                isEmpty = True
                break
        if isEmpty:
            break
            
    if not isEmpty:
        return True
        
    for num in range(1, 10):
        if isValid(board, row, col, num):
            board[row][col] = num
            if solveSudoku(board):
                return True
            board[row][col] = 0
    return False

# 读入数独
board = []
for _ in range(9):
    row = list(map(int, input().split()))
    board.append(row)

solveSudoku(board)

# 输出结果
for i in range(9):
    print(" ".join(map(str, board[i])))
```

```java []
import java.util.Scanner;

public class Main {
    public static boolean isValid(int[][] board, int row, int col, int num) {
        for(int x = 0; x < 9; x++) {
            if(board[row][x] == num) return false;
        }
        
        for(int x = 0; x < 9; x++) {
            if(board[x][col] == num) return false;
        }
        
        int startRow = row - row % 3, startCol = col - col % 3;
        for(int i = 0; i < 3; i++) {
            for(int j = 0; j < 3; j++) {
                if(board[i + startRow][j + startCol] == num) return false;
            }
        }
        return true;
    }
    
    public static boolean solveSudoku(int[][] board) {
        int row = -1, col = -1;
        boolean isEmpty = false;
        
        for(int i = 0; i < 9; i++) {
            for(int j = 0; j < 9; j++) {
                if(board[i][j] == 0) {
                    row = i;
                    col = j;
                    isEmpty = true;
                    break;
                }
            }
            if(isEmpty) break;
        }
        
        if(!isEmpty) return true;
        
        for(int num = 1; num <= 9; num++) {
            if(isValid(board, row, col, num)) {
                board[row][col] = num;
                if(solveSudoku(board)) return true;
                board[row][col] = 0;
            }
        }
        return false;
    }
    
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int[][] board = new int[9][9];
        
        // 读入数独
        for(int i = 0; i < 9; i++) {
            for(int j = 0; j < 9; j++) {
                board[i][j] = scanner.nextInt();
            }
        }
        
        solveSudoku(board);
        
        // 输出结果
        for(int i = 0; i < 9; i++) {
            for(int j = 0; j < 9; j++) {
                System.out.print(board[i][j]);
                if(j < 8) System.out.print(" ");
            }
            System.out.println();
        }
        
        scanner.close();
    }
}
```

---

## 算法及复杂度
- 算法：回溯算法（深度优先搜索）
- 时间复杂度：$\mathcal{O}(9^m)$，其中 $m$ 是空格的数量。对于每个空格都需要尝试9个数字。  
- 空间复杂度：$\mathcal{O}(m)$，其中 $m$ 是空格的数量，主要是递归调用栈的开销。
