## 题目
[题目链接](https://www.nowcoder.com/practice/a561ad77e7bb45679db2bd7317fded84?tpId=372&tqId=10979357&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

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
	int a,b;
	while(cin>>a>>b)
	{
		if(a==0&&b==0)
			break;
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
        int a,b;
        while(true){
            a=sc.nextInt();
            b=sc.nextInt();
            if(a==0&&b==0)
                break;
            System.out.println(a+b);
        }
    }
}
```
``` python []
while True:
    a,b=map(int,input().split())
    if(a==0 and b==0):
        break
    print(a+b)
```

---

## 算法及复杂度
- 算法：无。  
- 时间复杂度：$\mathcal{O}(t)$ 。  
- 空间复杂度：$\mathcal{O}(t)$ 。