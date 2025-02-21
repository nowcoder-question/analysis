## 题目
[题目链接](https://www.nowcoder.com/practice/54404a78aec1435a81150f15f899417d?tpId=37&tqId=36875&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

1. 题目要求找出链表倒数第 $k$ 个节点，可以使用快慢指针法：
   - 快指针先走 $k$ 步
   - 然后快慢指针同时走
   - 当快指针到达末尾时，慢指针正好在倒数第 $k$ 个位置
2. 需要注意的边界条件：
   - 链表长度 $n$ 的范围：$1 \leq n \leq 1000$
   - $k$ 的范围：$k \leq n$
   - 节点值的范围：$0 \leq \text{val}[i] \leq 10000$

---

## 代码

``` cpp []
#include <iostream>
using namespace std;

struct ListNode {
    int val;
    ListNode* next;
    ListNode(int x) : val(x), next(nullptr) {}
};

int main() {
    int n;
    while (cin >> n) {  // 处理多组测试用例
        // 创建链表
        ListNode* dummy = new ListNode(0);
        ListNode* curr = dummy;
        for (int i = 0; i < n; i++) {
            int val;
            cin >> val;
            curr->next = new ListNode(val);
            curr = curr->next;
        }
        
        // 读取k
        int k;
        cin >> k;
        
        // 快慢指针找倒数第k个节点
        ListNode* fast = dummy->next;
        ListNode* slow = dummy->next;
        
        // 快指针先走k步
        for (int i = 0; i < k - 1; i++) {
            fast = fast->next;
        }
        
        // 快慢指针同时走
        while (fast->next != nullptr) {
            fast = fast->next;
            slow = slow->next;
        }
        
        cout << slow->val << endl;
        
        // 清理内存
        while (dummy != nullptr) {
            ListNode* temp = dummy;
            dummy = dummy->next;
            delete temp;
        }
    }
    return 0;
}
```
``` java []
import java.util.*;

public class Main {
    static class ListNode {
        int val;
        ListNode next;
        ListNode(int val) {
            this.val = val;
        }
    }
    
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {  // 处理多组测试用例
            int n = sc.nextInt();
            
            // 创建链表
            ListNode dummy = new ListNode(0);
            ListNode curr = dummy;
            for (int i = 0; i < n; i++) {
                curr.next = new ListNode(sc.nextInt());
                curr = curr.next;
            }
            
            // 读取k
            int k = sc.nextInt();
            
            // 快慢指针找倒数第k个节点
            ListNode fast = dummy.next;
            ListNode slow = dummy.next;
            
            // 快指针先走k步
            for (int i = 0; i < k - 1; i++) {
                fast = fast.next;
            }
            
            // 快慢指针同时走
            while (fast.next != null) {
                fast = fast.next;
                slow = slow.next;
            }
            
            System.out.println(slow.val);
        }
    }
}
```
``` python []
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

# 处理多组测试用例
while True:
    try:
        # 读取输入
        n = int(input())
        values = list(map(int, input().split()))
        k = int(input())
        
        # 创建链表
        dummy = ListNode(0)
        curr = dummy
        for val in values:
            curr.next = ListNode(val)
            curr = curr.next
        
        # 快慢指针找倒数第k个节点
        fast = dummy.next
        slow = dummy.next
        
        # 快指针先走k步
        for _ in range(k - 1):
            fast = fast.next
        
        # 快慢指针同时走
        while fast.next:
            fast = fast.next
            slow = slow.next
        
        print(slow.val)
        
    except EOFError:
        break
```

---

## 算法及复杂度
- 算法：快慢指针法
- 时间复杂度：$\mathcal{O}(n)$ - 需要遍历一次链表
- 空间复杂度：$\mathcal{O}(1)$ - 只使用了常数额外空间
