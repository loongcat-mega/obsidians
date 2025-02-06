

- 特点：给定义一个闭区间，求区间内满足条件(某种性质)的数的总数量
- 技巧：
    - 类似于前缀和思想，\[l,r\]转换为\[0,l-1\],\[0,r\]问题，消去了下界判断
    - 从高位到低位填数，要分类讨论，划分为两类，$0\ to \ a_i-1$ 和$a_i$，保证填的数不超过R
    - 对数集从低位到高位做预处理-->打表
    - 再对数集从高位到低位做递推

注意点：
- 要不要前导0
- 何时剪枝
- 打表初始化按照什么规则


## 计数问题

给定一个数字`n`，统计从`1-n`中，数字`x`出现的次数，`n<=1e6`

对于小数据量，直接进行暴力循环`1-n`，估计时间复杂度为`O(K*n),K为1-n的平均位数`
洛谷P1980

暴力
```cpp
//  
// Created by dlut2102 on 2024/4/16.  
//  
#include<bits/stdc++.h>  
using namespace  std;  
int cnt(int n,int x)  
{  
    int ans=0;  
    while(n)  
    {  
        if(n%10==x)  
            ans++;  
        n/=10;  
    }  
    return ans;  
}  
  
int main()  
{  
    int  n,x,ans=0;  
    cin>>n>>x;  
    for(int i=1;i<=n;i++)  
        ans+=cnt(i,x);  
    cout<<ans<<"\n";  
}
```

## Windy数

不含前导零且相邻两个数字之差至少为 22 的正整数被称为 windy 数。windy 想知道，在 aa 和 bb 之间，包括 aa 和 bb ，总共有多少个 windy 数？
```cpp
//
// Created by dlut2102 on 2024/4/16.
//
//
// Created by dlut2102 on 2024/4/16.
//
#include<bits/stdc++.h>
using namespace  std;
//代表数位的最长长度
const int MAX_L=12;
//数位的每一位数
int R[MAX_L];
//f[i][j],表示一共有i位，且最高位为j的Windy数
int f[MAX_L][10];
void init()
{
    for(int i=0;i<10;i++)
        f[1][i]=1;
    //i代表位数,j代表第i位数，k代表j-1位
    for(int i=2;i<MAX_L;i++)
        for(int j=0;j<=9;j++)
            for(int k=0;k<=9;k++)
                if(abs(j-k)>=2)
                    f[i][j]+=f[i-1][k];
}
int dp(int n)
{
    if(!n) return 0;
    int cnt=0;
    ::memset(R,0,sizeof(int)*MAX_L);
    while(n)
    {
        R[++cnt]=n%10;
        n/=10;
    }
    int ans=0,last=-2;
    //答案是cnt位的
    //枚举位数
    for(int i=cnt;i>0;i--)
    {
        int now=R[i];
        //枚举每一位，最高位从1开始
        //不能j<=now
        for(int j=(cnt==i);j<now;j++)
            if(abs(j-last)>=2)
                ans+=f[i][j];
        if(abs(now-last)<2)
            break;
        last=now;
        if(i==1)
            ans++;
    }
    //答案小于cnt位
    for(int i=1;i<cnt;i++)
        for(int j=1;j<=9;j++)
            ans+=f[i][j];       
    return ans;
    
}
int main()
{
    int l,r;
    cin>>l>>r;
    init();
    cout<<dp(r)-dp(l-1)<<"\n";
}
```

## 计算x出现次数

```cpp
//
// Created by dlut2102 on 2024/4/16.
//
//
// Created by dlut2102 on 2024/4/16.
//
#include<bits/stdc++.h>
using namespace  std;
typedef long long int lli;
const int MAX_L=20;
int R[MAX_L];
lli cal(int l,int r)
{
    //[5,2]
    //[l,r]
    lli  ans=0;
    for(int i=l;i>=r;i--)
        ans=ans*10+R[i];
    return ans;
}
//递归形式
lli cnt(int N,int idx,int x)
{
    if(idx==0)
        return 0;
    lli ans=0;
    ans+=(cal(N,idx+1)-!x)*pow(10,idx-1);
    if(x==R[idx]) ans+=cal(idx-1,1)+1;
    else if (x<R[idx]) ans+=pow(10,idx-1);
    ans+=cnt(N,--idx,x);
    return ans;
}
//0-num
lli f(lli num,int x)
{
    if(!num)
        return 0;
    int N=0;
    while(num)
    {
        R[++N]=num%10;
        num/=10;
    }
    //位数不变，但枚举位数-1
    return cnt(N,N-!x,x);
}
lli dp(lli n,int x)
{
    if(n==0)
        return 0;
    int k=0;
    while(n)
    {
        R[++k]=n%10;
        n/=10;
    }
    lli ans=0;
    //如果要查询的是0，那么就不能从最高位枚举了
    for(int i=k-!x;i>=1;i--)
    {
    		//计算位置i之前的可能性
        ans+=(cal(k,i+1)-!x)*pow(10,i-1);
        //锁定位置i之前的数，计算位置i之后的可能性
        if(x==R[i]) ans+=cal(i-1,1)+1;
        else if(x<R[i]) ans+=pow(10,i-1);
    }
    return ans;
}
int main()
{
    lli l,r;
    cin>>l>>r;
    for(int i=0;i<10;i++)
        cout<<dp(r,i)-dp(l-1,i)<<" ";
    cout<<endl;
    for(int i=0;i<10;i++)
        cout<<f(r,i)-f(l-1,i)<<" ";
    
}
```

## 不要62

```cpp
//
// Created by dlut2102 on 2024/4/16.
//
#include<bits/stdc++.h>
using  namespace std;
const int MAX_L=12;
int R[MAX_L];
int f[MAX_L][10];
void init()
{
    for(int i=0;i<10;i++)
        if(i!=4)
            f[1][i]=1;
    for(int i=2;i<MAX_L;i++)
        for(int j=0;j<=9;j++)
        {
            for(int k=0;k<=9;k++)
                if(j==4||k==4||j==6&&k==2)
                    continue;
                else
                    f[i][j]+=f[i-1][k];
        }
}
int dp(int n)
{
    if(n==0)
        return 1;
    int N=0;
    while(n)
    {
        R[++N]=n%10;
        n/=10;
    }
    int ans=0,last=0;
    for(int i=N;i>=1;i--)
    {
        int now=R[i];
        //注意这里有没有前导0要求
        for(int j=0;j<now;j++)
        {
            //剪枝情况
            if(j==4||last==6&&j==2)
                continue;
            ans+=f[i][j];
        }
        //由数集树可知，当前面出现62或4时，就该剪枝了，不能再往下枚举
        if(now==4||last*10+now==62)
            break;
        //没有剪枝并且走到了最后一位
        if(i==1)
            ans++;
        last=now;
    }
    return ans;
}
int dp1(int n)
{
    if(n==0)
        return 1;
    int N=0;
    while(n)
    {
        R[++N]=n%10;
        n/=10;
    }
    int ans=0,last=0;
    for(int i=N;i>=1;i--)
    {
        int now=R[i];
        for(int j=(i==N);j<now;j++)
        {
            if(j==4||last==6&&j==2)
                continue;
            ans+=f[i][j];
        }
        if(now==4||last*10+now==62)
            break;
        if(i==1)
            ans++;
        last=now;
    }
    //或者扣掉最高位，只计算去掉最高位后的数
    //这里就没有限制条件了，why？？？？
    for(int i=N-1;i>=1;i--)
        for(int j=1;j<=9;j++)
            ans+=f[i][j];
    return ans;
}
int main()
{

    init();
    int l,r;
    cin>>l>>r;
    while(l|r)
    {
        cout<<dp(r)-dp(l-1)<<"\n";
        cout<<dp1(r)-dp1(l-1)<<"\n";
        cin>>l>>r;

    }
}
```