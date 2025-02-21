## 题目
[题目链接](https://www.nowcoder.com/practice/fe1e902e076b46dd82e7d42e12617893?tpId=372&tqId=10979371&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路
输入数组，循环求和。

---

## 代码

``` cpp []
#include <iostream>
using namespace std;
using ll=long long;
const int N=1010;
int a[N][N];
int main(void)
{
	ios::sync_with_stdio(false);
	cin.tie(0);
	ll sum;
	int t,n,m,i,j;
	cin>>t;
	while(t--)
	{
		sum=0;
		cin>>n>>m;
		for(i=1;i<=n;++i)
			for(j=1;j<=m;++j)
				cin>>a[i][j];
		for(i=1;i<=n;++i)
			for(j=1;j<=m;++j)
				sum+=a[i][j];
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
        int t,n,m,i,j;
        t=sc.nextInt();
        while(t-->0){
            n=sc.nextInt();
            m=sc.nextInt();
            int[][] a=new int[n+5][m+5];
            for(i=1;i<=n;++i)
                for(j=1;j<=m;++j)
                    a[i][j]=sc.nextInt();
            sum=0;
            for(i=1;i<=n;++i)
                for(j=1;j<=m;++j)
                    sum+=a[i][j];
            System.out.println(sum);
        }
    }
}
```
``` python []
t=int(input())
for _ in range(0,t):
    n,m=map(int,input().split())
    a=[list(map(int,input().split())) for i in range(0,n)]
    s=0
    for i in a:
        # s+=sum(i)
        for j in i:
            s+=j
    print(s)
```

---

## 算法及复杂度
- 算法：枚举。  
- 时间复杂度：$\mathcal{O}(\sum {n \cdot m})$ 。  
- 空间复杂度：$\mathcal{O}(\sum {n \cdot m})$ 。  
