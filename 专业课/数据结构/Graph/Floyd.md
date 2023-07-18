```c++
void Floyd(int **&Dis,int **&path)
{
	for(int i=0;i<N*N;i++)
	{
		Dis[i / N][i % N] = graph[i / N][i % N];
		path[i / N][i % N] = i / N;
	}
	for(int v=0;v<N;v++)
		for(int i=0;i<N;i++)
			for(int j=0;j<N;j++)
				if(Adj[i][j]>Adj[i][v]+Adj[v][j])
				{
					Adj[i][j]=Adj[i][v]+Adj[v][j];
					path[i][j]=path[v][j];
				}
}
```