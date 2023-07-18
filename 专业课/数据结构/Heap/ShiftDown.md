```c++
bool ShiftDown(int left)
{
	int i=left;
	int j=2*i+1;//左孩子
	int temp=heap[i];
	while(i<Cur)
	{
		if(j<Cur-1&&heap[j+1]>heap[j])//有右孩子，且右孩子比左孩子大
			j++;
		if(temp<heap[j])
		{
			heap[i]=heap[j];
			i=j;
			j=2*i+1;
		}
		else
			break;
	}
	heap[i]=temp;
	return true;
}
```