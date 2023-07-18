```c++
bool ShiftUp(int i)
{
	if(!i)
		return false;
	int j=(i-1)/2;//父节点
	while(heap[j]<heap[i])
	{
		swap(heap[i],heap[j]);
		ShiftDown(i);
		i=j;
		j=(i-1)/2;
	}
	return true;
}
```