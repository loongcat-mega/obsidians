12345$\times$ 4321
$f(x)=x^4+2x^3+3x^2+4x^1+5x^0$
$g(x)=4x^3+3x^2+2x^1+1x^0$
$h(x)=f(x)g(x)$

任意d阶多项式都有d+1个点唯一确定


差值工具：由点值表示到系数表示

```c++
float Lagrangian( Point*& p, int count, float num)
{
	Vf curx(count);
	Vf cury(count);
	for (int i = 0; i < count; i++)
	{
		curx[i] = p[i].x;
		cury[i] = p[i].y;
	}
	float ans = 0;
	for (int i = 0; i < count; i++)
	{
		float pi = 1;
		for (int j = 0; j < count; j++)
		{
			if (i == j)
				continue;
			pi *= (num - curx[j]) / (curx[i] - curx[j]);
		}
		ans += cury[i] * pi;
	}
	return ans;
}
```
