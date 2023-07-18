```c++
void Dijkstra(int n=0)
{
		/*
		* 1. [已访问][未访问]
		* 2. 满足条件 更新距离和前驱结点
		* 3. 找到刚刚更新的结点（s[i]==0）的最小值
		* 4. 
				1) 给最小值结点打上标记(s[i]==1),并从该节点入手开始搜索(i=index)
				2) 如果没有更新结点，继续访问下一个结点(i++)
		*/
	swap(Adj[0],Adj[n]);
	swap(name[0],name[n]);
	bool *s=new bool[N];//标记数组
	int *D=new int[N];//距离数组
	int *path=new int[N];//前驱结点
	for(int i=1;i<N;i++)
	{
		s[i]=0;
		D[i]=MAX;
		path[i]=-1;
	}
	s[0]=1;
	D[0]=0;
	path[0]=0;
	for(int i=0;i<N;)
	{
		if(s[i]==1)
		{
			int min=MAX+1,index=-1;
			for(int j=0;j<N;j++)
				if(s[j]==0)
					if((D[j]==MAX||D[j]>D[i]+Adj[i][j])&&Adj[i][j]!=MAX)
					{
						D[j]=D[i]+Adj[i][j];
						path[j]=i;
					}
			for(int i=0;i<N;i++)
				if(D[i]<min&&D[i]!=MAX&&s[i]==0)
				{
					min=D[i];
					index=i;
				}
			if(min!=MAX+1)
			{
				s[index]=1;
				i=index;
			}
			else
				i++;
		}
	}
	swap(Adj[0],Adj[n]);
	swap(name[0],name[n]);
}
```