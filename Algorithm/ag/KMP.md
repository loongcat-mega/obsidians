KMP是基于最长前缀与最长后缀相等的机制运行的
首先要建立一个ne数组，ne\[i\]存储的是从开始到第i个字符串组成的字符串，最长前后缀相等的长度
P与S比较的过程中，S指针始终不回退，P指针按照ne回退


## P3375 【模板】KMP

### 题目描述

给出两个字符串 s1s1​ 和 s2s2​，若 s1s1​ 的区间 [l,r][l,r] 子串与 s2s2​ 完全相同，则称 s2s2​ 在 s1s1​ 中出现了，其出现位置为 ll。  
现在请你求出 s2s2​ 在 s1s1​ 中所有出现的位置。

定义一个字符串 ss 的 border 为 ss 的一个**非 ss 本身**的子串 tt，满足 tt 既是 ss 的前缀，又是 ss 的后缀。  
对于 s2s2​，你还需要求出对于其每个前缀 s′s′ 的最长 border t′t′ 的长度。

### 输入格式

第一行为一个字符串，即为 s1s1​。  
第二行为一个字符串，即为 s2s2​。

### 输出格式

首先输出若干行，每行一个整数，**按从小到大的顺序**输出 s2s2​ 在 s1s1​ 中出现的位置。  
最后一行输出 ∣s2∣∣s2​∣ 个整数，第 ii 个整数表示 s2s2​ 的长度为 ii 的前缀的最长 border 长度。


### coding
```cpp
//
// Created by dlut2102 on 2024/4/15.
//
#include<bits/stdc++.h>
using namespace  std;
const int MAX_M=1e6;
//ne表示最长匹配的前后缀的长度
int ne[MAX_M+10];
void build(string P)
{
    ne[0]=0;
    for(int j=1;j<P.size();j++)
    {
        int k=ne[j-1];
        //如果当前字符 与上个字符位置对应的最长前缀的末尾不相同，就回退
        while(k&&P[j]!=P[k])k=ne[k-1];
        if(P[j]==P[k]) ne[j]=k+1;
        else ne[j]=0;
    }
}
int match(string P,string S,int st_idx=0)
{
    if(P.size()>S.size())
        return -1;
    int i=0,j=st_idx;
    while(i<P.size()&&j<S.size())
    {
        //如果匹配，双指针同时移动
        if(P[i]==S[j])
        {
            i++;
            j++;
        }
        //如果不匹配，并且P没有回退到0，就按照ne回退
        else if(i!=0) i=ne[i-1];
        //如果回退到0了，那么就该S指针移动
        else j++;
        if(i==P.size()) return j-i+1;
    }
    return -1;
}
int main()
{
    string P, S;
    
    cin>>S>>P;
    build(P);
    int idx=match(P,S);
    while(idx!=-1)
    {
        cout<<idx<<"\n";
        idx=match(P,S,idx);
    }
    for(int i=0;i<P.size();i++)
        cout<<ne[i]<<" ";
}
```

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240417164525.png)

感觉可能是函数调用这块浪费时间了