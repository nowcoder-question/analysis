## 题目
[题目链接](https://www.nowcoder.com/practice/72195b26cb0a4483a1f9fdf314ad63fa?tpId=372&tqId=10979351&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路
输入两个数字，输出它们的和。

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
	cin>>a>>b;
	cout<<a+b;
	return 0;
}
```
``` java []
import java.util.Scanner;
public class Main{
    public static void main(String[] args){
        Scanner sc=new Scanner(System.in);
        int a=sc.nextInt(),b=sc.nextInt();
        System.out.print(a+b);
    }
}
```
``` python []
a,b=map(int,input().split())
print(a+b)
```

---

## 算法及复杂度
- 算法：无。  
- 时间复杂度：$\mathcal{O}(1)$ 。  
- 空间复杂度：$\mathcal{O}(1)$ 。  
