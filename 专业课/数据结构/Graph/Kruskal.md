```c++
	void Kruskal()//最小生成树 按边查找 
	{
		UFsets ufs(N);
		CoorDis t, res;//保存结点及边权值的对象
		for (int i = 0; i < N * N; i++)
			if (graph[i / N][i % N] != MAX&& graph[i / N][i % N] != 0)
				t.InserHeap(i / N, i % N, graph[i / N][i % N]);//所有边入堆
		for (int i = 0;i!=N-1;i=res.Cur)//找到N-1条边就停止
		{
			Node temp;
			while (!t.IsEmpty())//找到库中最小的边
			{
				temp = t.mp.DelTop();
				if (ufs.Find(temp.i) !=  ufs.Find( temp.j))//最小边对应的结点不在同一个等价类中
				{
					res.Insert(temp.i, temp.j, temp.Distance);//将该边的结点权值加入结果库中
					ufs.Union(temp.i, temp.j);
					break;
				}
			}
		}
		for (int k = 0; k < res.Cur; k++)
			cout << res.head[k].i << "  " << res.head[k].j << "\t" << res.head[k].Distance << endl;
	}
```