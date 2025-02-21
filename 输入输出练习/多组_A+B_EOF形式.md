## 题目
[题目链接](https://www.nowcoder.com/practice/295063bf1bce4c2e819a8f18a5efcd20?tpId=372&tqId=10979354&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路
用 while 循环重复输入

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
		cout<<a+b<<'\n';
	return 0;
}
```
``` java []
import java.util.Scanner;
public class Main{
    public static void main(String[] args){
        Scanner sc=new Scanner(System.in);
        int a,b;
        while(sc.hasNext()){
            a=sc.nextInt();
            b=sc.nextInt();
            System.out.println(a+b);
        }
    }
}
```
``` python []
while True:
    try:
        a,b=map(int,input().split())
        print(a+b)
    except:
        break
```

---

## 算法及复杂度
- 算法：无。  
- 时间复杂度：$\mathcal{O}(t)$ 。  
- 空间复杂度：$\mathcal{O}(t)$ 。