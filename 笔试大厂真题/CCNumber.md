## 题目
[题目链接](https://www.nowcoder.com/practice/295008d558e546ffb61fc74fb75d4a27?tpId=182&tqId=325942&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

# 题解
## 难度：较难
## 知识点：动态规划，数学逻辑
## 题目分析

- CNumber就是一个山形状的整数，例如121，123421，18910这些数，都是先变大后变小，相邻数字间只能有大于或小于的关系，不能等于，CNumber最少3位数
- CCNumber就是两个紧邻的山构成的整数，意思就是这个数可以分为两个子整数，这两个子整数都是CNumber，能够分解到的CNumber之间不能共用数字，并且必须紧邻，CCNumber最少6位数
 **比如12121，那么分解为121和121共用了数字1，所以12121不是CCNumber** 
 **比如1211121，那么分解为121和121，但是中间多余一个1，所以1211121也不是CCNumber** 

 下表所示的数为CCNumber，注意一下 

 **13210132这个数，如果分解为13210和0132，但是0132不是CNumber，因为整数不以0开始，所以只有一种分法，分为13210和132**

 **再看看132354这个数，这道题说的是只要分解出来的整数满足CNumber之间不能共用数字，并且必须紧邻的要求，并没有要求分解CNumber的时候要分满，有些会认为132454分为132与2454那么就有共用数字，就不满足，其实是可以分为132和454就可以的**
| CCNumber | CNumber左 | CNumber右 |
| :----:| :----: | :----: |
| 121121 | 121 | 121 |
| 13210132 | 13210 | 132 |
| 132354 | 132 | 354 |

- CCNumber的score就是所有数字的和，例如121121的score=1+2+1+1+2+1=8

## 解题步骤
这道题的难点不在于判断一个数是不是CCNumber,而是找出[A,B]区间的所有CCNumber的最大score，如果遍历每个数来判断，复杂度过高，所以先来分析一下某些特殊区间的规律
{前缀+000...00}-{前缀+999...999}区间内的CCnumber的最大score能够很容易得到
例如：
000000-999999的最大score的CCnumber肯定是898898
300000-399999的最大score的CCnumber肯定是398898
1200000-1299999的最大score的CCnumber肯定是1298898
所以本题由区间分割，求解固定形式的区间中CCNumber的最大score两个步骤来求解

- 区间分割
把区间分为上述的特殊区间，先把A的前面扩充0与B同等长度，从最高位k开始：

  1)若B[k]=A[k],则移向下一位，即k=k+1

  2)若B[k]=A[k]+1,则将区间分为以下两段
1：{前k-1位公共前缀 B[k] 00...000}-B  
2: A-{前k-1位公共前缀 A[k] 99...999}

  3)若B[k]&gt;A[k]+1,除了将区间分为以下两段
1：{前k-1位公共前缀 B[k] 00...000}-B  
2:A-{前k-1位公共前缀 A[k] 99...999}
还需要分为以下段：
 **{前k-1位公共前缀 A[k]+1 00...000}-{前k-1位公共前缀 A[k]+1 99...999}
{前k-1位公共前缀 A[k]+2 00...000}-{前k-1位公共前缀 A[k]+2 99...999}
...
{前k-1位公共前缀 B[k]-2 00...000}-{前k-1位公共前缀 B[k]-2 99...999}
{前k-1位公共前缀 B[k]-1 00...000}-{前k-1位公共前缀 B[k]-1 99...999}**
可以发现，上述加粗的区间就是特殊区间，而1,2段区间则还得分别按照上述方法再分段

 ---
 例子：82560-133456，将A补充0与B一样长为082560-133456
 看最高位1=0+1，所以分为082560-099999与100000-133456

 对于082560-099999看第二位9=8+1，分段为082560-089999，090000-099999，接着按照同样方法继续分

 对于100000-133456看第二位3&gt;0+1, 除了分段为100000-109999,130000-133456(这两个段还得按照同样方法继续分段)，还分为特殊的分段区间110000-119999,120000-129999，这两个区间可以直接求解

  ---
 所以区间分割采用递归的方法来实现

- 特殊区间中CCNumber的最大score
特殊区间是{前缀+00..00}-{前缀+99..99}，所以得看前缀已经在两座山的哪个步骤了来得到后续数的组合中的CCNumber的最大socre

 将前缀处于的山的阶段分为7个阶段
 阶段划分
    1 前缀都为0的情况  意味着后续的组合成两座山
    2 前缀处于第一个山的上坡  意味着后续的组合可以再接着上坡或下坡
    3 前缀处于第一个山的下坡，但是只下了一步 
    4 前缀处于第一个山的下坡，下了大于1步    
    **分3阶段和4阶段的原因是，3的情况下，前缀的末尾数字不能当做第二座山的起点，不然两座山就共用数字了， 所以3阶段后续组合要么继续降，要么找个另外的起点开始上升；但是4的情况是，除了3阶段的可能，前缀的末尾数字能作为下一座山的起点**
    5 前缀的最后一个数是0或者最后一个数字大于等于倒数第二个数字 意味着后续组合必须开始第二座山的上坡
    6 前缀处于第二座山的上坡  意味着后续的组合可以再接着上坡或下坡
    7 前缀处于第二座山的下坡  意味着后续的组合只能接着下坡

    可以看出，特殊区间的CCNumber的最大socre由前缀的阶段stage，前缀的最后一个数字，区间后续可组合数字的位数来决定，由于该题目需要求多组区间，并且在每组区间的递归中要不断求解特殊区间的解，重复度过高，所以在开始计算前先将所有区间的所有情况进行求解，到时计算的时候直接取值即可，存于全局数组中vector<vector<vector<int>&gt; &gt; T(8,vector<vector<int>&gt;(10,vector<int>(20,-1)))
其中
第一维8为前缀的8个阶段，stage=-1，前缀已经导致不能构成CCNumber了
第二维10，前缀的末尾数字0~9，一共10个
第三维20，意思是后续可组合的数字位数，本题中A&lt;=B&lt;2^64，那么10进制最多20位数字，所以这里用20位

 依次求解stage=7,6,5,4,3,2,1的，由于stage=i的后续是stage=i+1，所以求解stage=i的最优用到了stage=i+1的最优结果，此处为动态规划的知识

完整代码为：
```
#include <string>
#include <iostream>
#include <vector>
#include <time.h>
using namespace std;
const int num_digit = 10;//数字位数
   
const int max_bit = 20;//整数最大位数
const int max_size = max_bit+1;
   
vector<vector<vector<int>&gt; &gt; T(8,vector<vector<int>&gt;(num_digit,vector<int>(max_size,-1)));//T[k][i][num] 阶段k 当末尾为i时 使用num个数字填充的最大Score  -1表示无法填充


void preprocessing(int stage){
    if(stage==7){
        //阶段7 只允许降
        for(int i=0;i&lt;=9;i++){
            for(int k=1;k&lt;=max_bit;k++){
                if(k&gt;i){
                     break;
                }else{
                    if(k==1) {
                        T[7][i][k]=i-k;//这是第一个后续的最大数，比如i=9，则这个数为8
                    }
                    //  i-1,i-2,...,0相加
                    else {
                        T[7][i][k]=T[7][i][k-1]+i-k;
                    }
                }
            }
        }
    }
    if(stage==6){
        //阶段6 可选升/降
        for(int i=2;i&lt;=9;i++){
            for(int k=1;k&lt;=max_bit;k++){
                //只有一个数那么只能降
                if(k==1){
                        T[6][i][k]=i-k;
                    continue;
                }
                int ans = -1;
                int sum = 0;
                 //尝试升  j是可能达到的山顶
                for(int j=9;j&gt;=i+1;j--){
                    //根据升到达山顶的步数，判断总步数是否够，因为要下山最少留一步
                    if(k&lt;=(9-j+1)) break;
                    sum+=j;
                    int t = T[7][9][k-(9-j+1)];//升后最后一定是9
                    if(t==-1) continue;
                    if(ans<sum+t){ ans="-1;" } 或者直接降 if(ans<t[7][i][k]){ t[6][i][k]="ans;" if(stage="=5){" 节点5 必须升 for(int i="0;i<=9;i++){" k="1;k<=max_bit;k++){" int sum="0;" 升到几 需得尝试 j="9;j">=i+1;j--){
                    if(k&lt;=(9-j+1)) break;//
                    sum+=j;
                    int t = T[6][9][k-(9-j+1)];
                       
                    if(t==-1) continue;
                    if(ans<sum+t){ ans="-1;" } if(k!="1" and break; t[5][i][k]="ans;" if(stage="=4){" 阶段4 可选降 或是下一轮 或自选一起点进入下一轮 for(int i="0;i<=9;i++){" k="1;k<=max_bit;k++){" int sum="0;" 降到几 需得尝试 j="i-1;j">=0;j--){
                    if(k&lt;=(i-j)) break;
                    sum+=j;
                    int t = T[4][j][k-(i-j)];//因为k-(i-j)为更少的位数，已经在上一个循环计算出来了
                    if(t==-1) continue;
                    if(ans<sum+t){ ans="-1;" } 或以当前节点为首 进入下一轮,以i开始的数的山 if(i!="0){" if(t[5][i][k]!="-1" and ans<t[5][i][k]){ 自选下一个位的值 序列首不能是0 for(int j="i-1;j" if(k="=1)" break; if(t[5][j][k-1]="=-1)" continue; if(ans<j+t[5][j][k-1]){ t[4][i][k]="ans;" if(stage="=3){" 阶段3 只降了一次 可选继续降 或 自选一个起点进入下一轮起点阶段(要借一位) i="0;i<=9;i++){" k="1;k<=max_bit;k++){" int sum="0;" 降到几 需得尝试>=0;j--){
                    if(k&lt;(i-j+3)) break;
                    sum+=j;
                    int t = T[4][j][k-(i-j)];
                    if(t==-1) continue;
                    if(ans<sum+t){ ans="-1;" } 自选下一个位的值 序列首不能是0 for(int j="i-1;j" if(k="=1)" break; if(t[5][j][k-1]="=-1)" continue; if(ans<j+t[5][j][k-1]){ t[3][i][k]="ans;" if(stage="=2){" 阶段2 已经有过升序，可选继续升或降 i="0;i<=9;i++){" k="1;k<=max_bit;k++){" int sum="0;" 降到几 需得尝试>=0;j--){
                    if(k&lt;(i-j+3)) break; 
                    sum+=j;
                    int t = T[3][j][k-(i-j)];
  
                    if(t==-1) continue;
                    if(ans<sum+t){ ans="sum+t;" } sum="0;" 升到几 需得尝试 for(int j="9;j">=i+1;j--){
                    if(k&lt;=(9-j+1)) break;
                    sum+=j;
                    int t = T[3][9][k-(9-j+1)];
                    if(t==-1) continue;
                    if(ans<sum+t){ ans="-1;" } t[2][i][k]="ans;" if(stage="=1){" 1 表示第一序列的起始 后必须接着一个升序 for(int i="0;i<=9;i++){" k="1;k<=max_bit;k++){" int sum="0;" j="9;j">=i+1;j--){
                    if(k&lt;=(9-j+1)) break;
                    sum+=j;
                    int t = T[2][9][k-(9-j+1)];
                    if(t==-1) continue;
                    if(ans<sum+t){ ans="sum+T[2][9][k-(9-j+1)];" } if(i="=n-1)" and k="=5){" t[1][i][k]="ans;" 这个函数是判断前缀s当前处于什么阶段 返回stage，stage="-1表示前缀阶段已经导致整个数是不可能为CCNumber了" * 阶段划分 1 表示第一序列的起始 后必须接着一个升序 2 已经有过升序，可选继续升或降 3 只降了一次 可选继续降 或 自选一个起点进入下一轮起点阶段(要借一位) 4 继续降(如果后续位数过多) 下一轮起点阶段 5 第二轮的起点 必须升 6 第二轮升序 可选升 降 7 第二轮降序 int cal_stage(string s){ n="s.length();" stage="1就是说前缀都为0" i="0;" while(i<n s[i] i++; return stage; if(i<n-1>=s[i+1]) return -1;//没有增
    stage=2;//stage=2表面前缀已经遇到一次升序
       
       
    while(i<n-1 and s[i]<s[i+1]) i++; 升序 升序过后，i为第一个极高点 循环结束后需要判定是因为到了末尾还是因为遭遇了 降或者平 if(i="=n-1)" return stage; 末尾 返回阶段2 if(s[i]="=s[i+1])" -1; 遇到平就错误 int j="i;" while(i<n-1 s[i]>s[i+1]) i++;//降序
    //只降了一次
    if(i-j==1){
        //降一次 变为阶段3
        stage=3; 
        if(i==n-1) return stage;
           
        j = i+1;
        stage = 5;
           
    }else{
        stage=4;
        if(i==n-1) return stage;//如果结束 以阶段4返回
        //否则转为5,此时i或i+1  都可能是下一轮起点
        stage=5;
        //如果i是0  或者  两者相等 则i+1必然是起点
        if(s[i]=='0' or s[i]==s[i+1]){
            j=i+1;
        }else j=i;//否则i就是起点
    }
    if(j==n-1) return stage;
    i=j;//i就是第二座山的起点
    //第二座山开启
    if(s[i]=='0' or s[i]&gt;=s[i+1]) return -1;//首为0 或降、平序
    stage = 6;
       
    while(i<n-1 and s[i]<s[i+1]) i++; 第二序列 升序 if(i="=n-1)" return stage; 末尾 返回阶段6 if(s[i]="=s[i+1])" -1; 遇到平 遇到减 转为阶段7 stage="7;" while(i<n-1 s[i]>s[i+1]) i++;
       
    if(i==n-1) return stage;//到末尾一直是减 返回阶段7
    //再度遭遇平或升
    return -1;
}
   
int score;//这个是全局的score，存了最大的score
//此时A,B前[0-k]位相同 后面A全为0  B全为9的特殊区间求解
void count(string A,string B,int k,int prefix_value){
    int n = A.length();
    int i=0;
    while(i<n and a[i]="='0')" i++; 判定公共前缀的状态 string prefix="A.substr(i,(k-i)+1);" int stage="cal_stage(prefix);" v="A[k]-'0';" if(stage="=-1)" return; if(t[stage][v][n-k-1]="=-1);" now="prefix_value+T[stage][v][n-k-1];" if(now>score){
        score = now;
    }
}
   
//计算[A,B]中包含CCNumber的整数的最大Score
//k是前缀位数，prefix_score是前缀的score
void cal(string A,string B,int k,int prefix_score){
    int n = A.length();
    int i=0;
    //这里纯粹是处理数据，A B前缀都是0的部分处理掉
    while(i<n and i<k (a[i]="='0'" b[i]="='0')){" k--; i++; n--; } if(i!="0){" a="A.substr(i);" b="B.substr(i);" 如果a和b将共同前缀0去掉后的长度小于6，就没有ccnumber了 if(n<6) return ; 如果a="B那么直接判断A是不是CCNumber，用cal_stage函数判断" 如果是则不用后续的区间分解了 if(a="=B){" if(n>=6 and cal_stage(A)==7){
            int sum = 0;
            for(auto c:A){
                sum+=c-'0';
            }
            if(sum&gt;score) score = sum;
        }
        return ;
    }
    //这是是为了加速计算的部分，如果当前的前缀加上后面是000..00-99..99的区间情况得到
    //的score如果小于前面计算得到的最大score，则不需要继续分区间了，最大的已经找着了
    string prefix = "";
    int stage = -1;
    if(k!=0){
        prefix = A.substr(0,k);
          
        stage = cal_stage(prefix);
          
        if(stage==-1) {
            return;
        }
          
        if(prefix_score+T[stage][A[k-1]-'0'][n-k]&lt;=score){
            return ;
        }
    }
    //前缀都包括完了则返回
    if(k==n-1) return ; 
      
       
    //这里是如果k位相等，则前缀加一位，k=k+1即可
    if(A[k]==B[k]){
        cal(A,B,k+1,prefix_score+A[k]-'0');
        return ;
    }
    //这里是B[k]-A[k]&gt;1多得到的特殊区间，可以直接求解
    if(B[k]-A[k]&gt;1){
        string ma(A),mb(B);
        for(int i=k+1;i<n;i++){ ma[i]="'0';" mb[i]="'9';" } for(int mk="A[k]+1;mk<=B[k]-1;mk++){" ma[k]="mk;mb[k]=mk;" 特殊区间的求解函数count count(ma,mb,k,prefix_score+mk-&#39;0&#39;); 这是b[k]-a[k]>1或B[k]-A[k]=1得到的区间段，还得通过下面方法递归求解
    string na(B),nb(A);
    for(int i=k+1;i<n;i++){ na[i]="'0';" nb[i]="'9';" } cal(na,b,k+1,prefix_score+b[k]-&#39;0&#39;); cal(a,nb,k+1,prefix_score+a[k]-&#39;0&#39;); int main(){ n; cin>&gt;N;
    for(int i=7;i&gt;=1;i--){
        preprocessing(i);
    }
    for(int i=0;i&lt;=9;i++){
        for(int k=1;k&lt;=max_bit;k++){
            for(int t=7;t&gt;=1;t--){
                T[0][i][k]=max(T[t][i][k],T[0][i][k]);
            }
        }
    }
       
    for(int k=1;k&lt;=N;k++){
        string A,B;
        cin&gt;&gt;A&gt;&gt;B;
           
        score = 0;
        //将A补0，填充到与B同长
        int an = A.length(),bn = B.length();
        for(int i=an;i</n;i++){></n;i++){></n></n></n-1></n-1></sum+t){></sum+t){></sum+t){></sum+t){></sum+t){></sum+t){></sum+t){></int></vector<int></vector<vector<int></time.h></vector></iostream></string></int></vector<int></vector<vector<int>