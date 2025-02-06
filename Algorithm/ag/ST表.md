Sparse **Table**
主要解决RMQ(区间最大最小值问题),应用倍增思想，实现O(nlogn)预处理，O(1)查询
一次建表，多次查询
静态表，建成后不可修改
**不支持修改，区间/单点查询**

## 预处理：
倍增递推法：用两个等长小区间拼凑成一个大区间
```txt
f[i][j]:以第i个数为起点，长度2^j的区间中最值
```
```cpp
//f[i][j]=max(f[i][j-1],f[i+(j-1)<<1][j-1]);
for(int j=1;j<=MAX_LOG;j++)  
    for(int i=1;i+(1<<j)-1<=N;i++)  
        st[i][j]=max(st[i][j-1],st[i+(1<<(j-1))][j-1]);
```

## 查询

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240327095959.png)

对查询区间做切割
```cpp
max(f[l][x],f[r-1<<x+1][x]);
```

## coding
```cpp
#include<bits/stdc++.h>  
using  namespace std;  
#define MAX_LOG 18  
const int MAX_N=1e5+10;  
vector<int>lg2(MAX_N,0);  
int st[MAX_N][MAX_LOG];  
void init()  
{  
    lg2[1]=0;lg2[2]=1;  
    for(int i=3;i<=MAX_N;i++)  
        lg2[i]=lg2[i>>1]+1;  
}   
int main()  
{  
    int N,M;  
    cin>>N>>M;  
    for(int i=1;i<=N;i++)  
        cin>>st[i][0];  
    for(int j=1;j<=MAX_LOG;j++)  
        for(int i=1;i+(1<<j)-1<=N;i++)  
            st[i][j]=max(st[i][j-1],st[i+(1<<(j-1))][j-1]);  
    int l,r;  
    for(int i=0;i<M;i++)  
    {  
        cin>>l>>r;  
        //int k=lg2[r-l+1];  
        int k=(int)log2(r-l+1);  
        //cout<<max(st[l][k],st[r-(1<<k)+1][k])<<endl;  
        cout<<max(st[l][k],st[r-(1<<k)+1][k])<<"\n";  
    }  
}
```


把endl换成"\\n" 就能通过

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240407104947.png)
