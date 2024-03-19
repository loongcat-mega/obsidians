## typora

1. 角标

   输入A+B=C
   $$
   A+B=C
   $$
   上角标^
   $$
   a^2+b^2=c^2
   $$
   下角标_
   $$
   S_n=a_1+a_2+a_3+……+a_n
   $$

   $$
   \sum_{i=0}^{5}=
   $$

   
$$\oint_LE\cdot dl=-{{\int\kern{-7pt}\int}\kern{-21mu}\bigcirc}
$$
$\frac {\partial Loss } {\partial C }$

$$
\prod_{i=1}^{5}=
$$
​	2. 运算符


$$
\sum k
$$
空格：\\\quad (后有空格)
$$
A   B
$$

$$
A\quad B
$$



点乘 \cdot 

\cdots 点乘省略号 

乘号 \times
$$
+-
A\cdot B \quad     C\cdots B
$$

$$
1\times2\times3\times4=
$$

$$
1.4\times10^{-5}
$$

分式 \frac
$$
\frac{1}{2}
$$
\pm 加减
$$
x_{1,2}=\frac{-b\pm \sqrt{b^2-4ac}}{2a}
$$
 异或 \oplus    同或 \odot
$$
A\oplus B \oplus C=A\odot B\odot C
$$
3.关系符
$$
<>=
$$
大于等于 \geq  小于等于 \leq   约等于\apprax  不等于 \neq
$$
a\geq b
$$

$$
a\leq b
$$

$$
a\approx b
$$

$$
a\neq b
$$

集合属于 \in
$$
a\in A
$$
交集\cap
$$
A\cap B
$$
并集\cup
$$
A\cup B
$$

$$
a\in A\cup B且a\in A\cap B
$$

对于形式如下
$$
\lim _{x\rightarrow\infty}\frac{a_nx^n+a_{n-1}x^{n-1}+\dots+a_1x+a_0}{b_mx^m+b_{m-1}x^{m-1}+\dots+b_1x+b_0}
$$
的分式求x在正无穷处的极限时，同时除分母的最大次幂（根号同理），如下
$$
\lim _{x\rightarrow\infty}\frac{a_nx^n+a_{n-1}x^{n-1}+\dots+a_1x+a_0}{b_mx^m+b_{m-1}x^{m-1}+\dots+b_1x+b_0}=\lim _{x\rightarrow\infty}\frac{a_n\frac{x^n}{x^m}+a_{n-1}\frac{x^{n-1}}{x^{m}}+\dots +a_1\frac{x}{x^{m}}+a_0\frac{1}{x^{m}}}{b_m+b_{m-1}x^{-1}+\dots +b_1x^{1-m}+b_0x^{-m}}
$$

$$
\lim _{x\rightarrow\infty}\frac{a_n\frac{x^n}{x^m}+a_{n-1}\frac{x^{n-1}}{x^{m}}+\dots +a_1\frac{x}{x^{m}}+a_0\frac{1}{x^{m}}}{b_m+b_{m-1}x^{-1}+\dots +b_1x^{1-m}+b_0x^{-m}}
$$



此时，要分情况讨论

1. 当n>m时

   n-m>0;故分子前n-m+1项为x的正次幂，之后项为x的负次幂（分数），故前n-m+1项求极限时极限为正无穷，后面项由于时分数（x的次幂在分母上），其极限为0

   因此原式在正无穷处的极限为正无穷

2. 当n<m时

   与1正好反过来，其极限为0

3. 当n=m时

   处理后的式子变为
   $$
   \lim _{x\rightarrow\infty}\frac{a_n+a_{n-1}x^{-1}+\dots +a_1{x^{1-n}}+a_0x^{-n}}{b_n+b_{n-1}x^{-1}+\dots +b_1{x^{1-n}}+b_0x^{-n}}
   $$
   

$$
\lim _{x\rightarrow\infty}\frac{a_n+[a_{n-1}x^{-1}+\dots +a_1{x^{1-n}}+a_0x^{-n}]}{b_n+[b_{n-1}x^{-1}+\dots +b_1{x^{1-n}}+b_0x^{-n}]}
$$

上下两个[]内的式子在x为正无穷的极限值为0

故
$$
\lim _{x\rightarrow\infty}\frac{a_n+[a_{n-1}x^{-1}+\dots +a_1{x^{1-n}}+a_0x^{-n}]}{b_n+[b_{n-1}x^{-1}+\dots +b_1{x^{1-n}}+b_0x^{-n}]}=\frac{a_n}{b_n}
$$
反斜杠\vert
$$
P(A\vert B)=\frac{P(AB)}{P(A)}
$$

4. 希腊字母

   \alpha

   \beta

   \epsilon

   \theta

   \lambda

   \rho

   \phi

   \delat

   

   
   $$
   \alpha\quad  \beta\quad  \epsilon\quad  \theta \quad  \lambda \quad \rho \quad \phi\quad \delta \Delta
   $$
   

向量 \vec \overrightarrow
$$
\vec{A}=\overrightarrow{A}
$$
推出\Longrightarrow
$$
A\Longrightarrow{B}
$$
任意\forall

存在\exists

因为 \because

所以\therefore




$$
\forall\quad \exists\quad \because \quad \therefore
$$

$$
数列极限的定义\\
{a_n}为一数列，A为一实数。若\forall\epsilon>0,\exists N\in N_+,使得当n>N时，均有|a_n-A|<\epsilon,则称a_n的极限为A,\\记作\lim_{n\rightarrow\infty}a_n=A,或a_n\rightarrow A(n\rightarrow \infty)
$$


$$
复合函数的极限
\\
若\lim_{x\rightarrow x_0}g(x)=u_0，\lim_{u\rightarrow u_0}f(u)=f(u_0),则
\\
\lim_{u\rightarrow u_0}f(g(x))=f(u_0)
$$

$$
例如(错误示范)\\\lim_{x\rightarrow0}sin{(2x+1)}\neq\lim_{x\rightarrow0}(2x+1)=1
$$

$$
正确解\\
\lim_{x\rightarrow0}sin{(2x+1)}=\lim_{x\rightarrow{\lim_{x\rightarrow0}(2x+1)}}sinx=\lim_{x\rightarrow1}sinx=sin1
$$

$$
\lim_{x\rightarrow3}\frac{sin(x+3)}{x}=
$$

数列极限的性质

性质1.唯一性
$$
若数列{a_n}收敛，则它的极限唯一。
$$

$$
\epsilon-N语言中，\epsilon本质上是一个变量，\forall\epsilon是一个无穷小量，只是表示一个无穷小量,\\
而\forall\epsilon都能存在一个N，使得|a_n-A|<\epsilon_0,\epsilon_0与\epsilon本质上是都是无穷小量
$$

$$\int_{-\frac{\theta}{2}}^{\frac{\theta}{2}}$$

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefinedPasted%20image%2020230117184804.png)
$$\begin{align}
\frac{14}{\sqrt 3-1}&=\frac{14*(\sqrt3+1)}{(\sqrt 3-1)*(\sqrt3+1)}\\
&=\frac{14*(\sqrt3+1)}{2}\\
&=7(\sqrt3+1)
\end{align}
$$
$$
    \left.
        \begin{array}{l}
            \text{if $n$ is even:} & n/2 \\
            \text{if $n$ is odd:} & 3n+1 \\
        \end{array}
    \right\}
    =f(n)
$$
