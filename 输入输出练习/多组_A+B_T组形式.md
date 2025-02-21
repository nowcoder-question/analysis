## 题目
[题目链接](https://www.nowcoder.com/practice/cc937d32225340469bfb60a0797bad77?tpId=372&tqId=10979356&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路
使用 while 循环重复输入

---

## 代码

``` cpp []
#include <iostream>
using namespace std;
int main(void)
{
	ios::sync_with_stdio(false);
	cin.tie(0);
	int t,a,b;
	cin>>t;
	while(t--)
	{
		cin>>a>>b;
		cout<<a+b<<'\n';
	}
	return 0;
}
```
``` java []
import java.util.Scanner;
public class Main{
    public static void main(String[] args){
        Scanner sc=new Scanner(System.in);
        int t=sc.nextInt(),a,b;
        while(t-->0){
            a=sc.nextInt();
            b=sc.nextInt();
            System.out.println(a+b);
        }
    }
}
```
``` python []
t=int(input())
for _ in range(0,t):
    a,b=map(int,input().split())
    print(a+b)
```

---

## 算法及复杂度
- 算法：无。  
- 时间复杂度：$\mathcal{O}(t)$ 。  
- 空间复杂度：$\mathcal{O}(t)$ 。