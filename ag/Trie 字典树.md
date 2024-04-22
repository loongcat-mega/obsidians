
可以维护字符串，二进制串
从根节点到叶子结点的每一条路径都代表元素
两个操作：
- 建树
- 查询

时间复杂度O(n)

```cpp
//
// Created by dlut2102 on 2024/4/17.
//
#include<bits/stdc++.h>
using namespace std;
const int MAX_N=1e5+10;
//孩子结点数组 每个ch[i][x]代表他的孩子结点值
int ch[MAX_N][26];
//元素的出现次数
int cnt[MAX_N];
//结点计数值
int idx;
void insert(string s)
{
    int p=0;
    for(int i=0;i<s.size();i++)
    {
        int now=s[i]-'a';
        if(!ch[p][now]) ch[p][now]=++idx;
        p=ch[p][now];
    }
    cnt[p]++;
}
int query(string s)
{
   int p=0;
   for(int i=0;i<s.size();i++)
   {
       int now=s[i]-'a';
       if(!ch[p][now]) return 0;
       p=ch[p][now];
   }
   return cnt[p];
}
int main()
{
    string s;
    for(int i=0;i<10;i++)
    {
        cin>>s;
        insert(s);
    }
    while(1)
    {
        cin>>s;
        cout<<query(s)<<"\n";
    }
}
```


## 01Trie


维护的01串，用于解决最大异或问题


在给定的 N 个整数 A1，A2……AN 中选出两个进行 xor（异或）运算，得到的结果最大是多少？

输出一个整数表示答案。
```cpp
//
// Created by dlut2102 on 2024/4/17.
//
#include<bits/stdc++.h>
using namespace std;
const int MAX_N=1e5+10;
int ch[31*MAX_N][2];
int idx;
void insert(int n)
{
    int p=0;
    //这里取30，是因为int类型最高位是符号位
    for(int i=30;i>=0;i--)
    {
        int now=(n&(1<<i))>>i;
        if(!ch[p][now]) ch[p][now]=++idx;
        p=ch[p][now];
    }
}
int query(int n)
{
    //查询函数查询的是与当前n值异或最大的值，根据树的状态来查询
    int p=0;
    //注意这里，res为n
    int ans=n;
    for(int i=30;i>=0;i--)
    {
        int now=n>>i&1;
        //如果有相反的路径存在
        if(ch[p][!now])
        {
            //那么直接将对应位置改为1
            ans|=1<<i;
            //往对面方向走
            p=ch[p][!now];
            
        }
        else
        {
            //将对应位置改为0
            ans&=~(1<<i);
            p=ch[p][now];
        }
    }
    return ans;
    
}
int main()
{
    int n;
    cin>>n;
    vector<int>vals(MAX_N);
    for(int i=0;i<n;i++)
    {
        cin>>vals[i];
        insert(vals[i]);
    }
    int res=0;
    for(int i=0;i<n;i++)
        res=max(res,query(vals[i]));
    cout<<res;
}
```