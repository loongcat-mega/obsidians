```c++
void Prim()
{
		/*
	* 1. [已访问][未访问]
	* 2. 有边就入库（最小堆)
	* 3. 找到库中最小边，将其加入U中，打上已访问的标记，并加该节点及边加入res中
	* 4. 从刚刚加入的结点开始访问（必定加入新结点）
	* 5. 如果边数==结点数-1 退出循环
	*/
	bool *U=new bool [N];
	for(int i=0;i<N;i++)
		U[i]=0;
	U[0]=1;
	CoorDis t,res;
	for(int i=0;res.Cur!=N-1;i=res.head[Cur-1].j)
		if(U[i]==1)
		{
			for(int j=0;j<N;j++)
				if(U[j]==0)
					if(Adj[i][j]!=MAX)
						t.InserHeap(i, j, Adj[i][j])
			Node temp;
			while(1)
			{
				temp=t.mp.Deltop();
				if(U[temp.i]!=1||U[temp.j]!=1)
				{
					res.Insert(temp);
					U[temp.j]=1;
					break;
				}
			}
		}
}
```

