## 题目
[题目链接](https://www.nowcoder.com/practice/6652094c6a0e4381911b541ed664d8b7?tpId=372&tqId=10979414&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路
后台有spj代码，能对同学们的输出数据进行校验，符合条件即可通过。  
附赠 spj 代码  

``` cpp
#include <iostream>
#include <fstream>
#include <string>
#include <cctype>
#include <algorithm>
#include <cmath>
using namespace std;
int main(void)
{
	ifstream in,out,user_out;
	in.open("input");
	out.open("output");
	user_out.open("user_output");
	string s,t;
	if(!(user_out>>s))
		return 1;
	out>>t;
	if(fabs(stod(s)-stod(t))>1e-3)
		return 1;
	if(user_out>>s)
		return 1;
	return 0;
}
```

---

## 代码

``` cpp []
#include <iostream>
#include <iomanip>
#include <cmath>
using namespace std;
using ld=long double;
const ld pi=acosl(-1);
int main(void)
{
	ios::sync_with_stdio(false);
	cin.tie(0);
	ld r;
	cin>>r;
	cout<<fixed<<setprecision(6)<<r*r*pi;
	return 0;
}
```
``` java []
import java.util.Scanner;
import java.math.*;
public class Main{
    public static void main(String[] args){
        Scanner sc=new Scanner(System.in);
        int n=sc.nextInt();
        System.out.printf("%.6f",n*n*Math.PI);
    }
}
```
``` python []
import math
r=int(input())
print('%.6f'%(r*r*math.pi))
```

---

## 算法及复杂度
- 算法：无。  
- 时间复杂度：$\mathcal{O}(1)$ 。  
- 空间复杂度：$\mathcal{O}(1)$ 。