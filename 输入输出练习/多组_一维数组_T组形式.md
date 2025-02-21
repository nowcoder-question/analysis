## 题目
[题目链接](https://www.nowcoder.com/practice/90cdc8e31ef74b6e9fd44c1e03077b57?tpId=372&tqId=10979367&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路
输入数组，循环求和。

---

## 代码

``` cpp []
#include <iostream>
using namespace std;
using ll=long long;
const int N=100100;
int a[N];
int main(void)
{
	ios::sync_with_stdio(false);
	cin.tie(0);
	ll sum;
	int t,n,i;
	cin>>t;
	while(t--)
	{
		sum=0;
		cin>>n;
		for(i=1;i<=n;++i)
			cin>>a[i];
		for(i=1;i<=n;++i)
			sum+=a[i];
		cout<<sum<<'\n';
	}
	return 0;
}
```
``` java []
import java.util.Scanner;
public class Main{
    public static void main(String[] args){
        Scanner sc=new Scanner(System.in);
        long sum;
        int t,n,i;
        t=sc.nextInt();
        while(t-->0){
            n=sc.nextInt();
            int[] a=new int[n+5];
            for(i=1;i<=n;++i)
                a[i]=sc.nextInt();
            sum=0;
            for(i=1;i<=n;++i)
                sum+=a[i];
            System.out.println(sum);
        }
    }
}
```
``` python []
t=int(input())
for _ in range(0,t):
    n=int(input())
    a=list(map(int,input().split()))
    # print(sum(a))
    s=0
    for i in a:
        s+=i
    print(s)
```

---

## 算法及复杂度
- 算法：枚举。  
- 时间复杂度：$\mathcal{O}(\sum n)$ 。  
- 空间复杂度：$\mathcal{O}(\sum n)$ 。  
