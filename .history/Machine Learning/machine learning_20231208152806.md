
# supervised learning 

learning input to output or x to y mappings

learn from data labeled with the "right answers"

## Regression

predict a number from infinitely many possible numbers

### Linear Regression

Notations:
- m: number of training examples
- x ,y
- $(x^{(i)},y^{(i)})$ 

fitting a straight line to your data

To train you model ,you feed the training set both the input features and the output targets to your learning algorithm

$$
f_{w,b}(x)=wx+b
$$
In order to implement linear regression ,the first key step is to define something called a cost function:

find $w,b$ ,$\hat y^{(i)}$  is close to  $y^{(i)}$ for all $(x^{(i)},y^{(i)})$ 


#### Cost Function

measure how well a line fits the training data

$$
J(w,b)=\frac{1}{2m}\sum_{i=1}^m(\hat y^{(i)}-y^{(i)})^2
$$

goal of linear regression:

$minimize_{w,b}J(w,b)$ 

#### Gradient descent

having function $J(w,b)$
want $min_{w,b}J(w,b)$

Outline:
- start with some w,b
- keep changing w ,b to reduce $J(w,b)$
- until we settle at or near a minimum

$$
w=w-\alpha \frac{\partial{J(w,b)}}{\partial w}=w-\alpha \frac1m \sum_{i=1}^m (\hat{y}^{(i)}-y^{(i)})*x^{(i)}
$$

$$b=b-\alpha \frac{\partial{J(w,b)}}{\partial b}=b-\alpha \frac1m \sum_{i=1}^m (\hat{y}^{(i)}-y^{(i)})$$



- $\alpha$ learning rate ,it basicall control how big of step you take downhill
    - too small : gradient descent may be slow
    - too large: overshot, never reach minimum,fail to converge ,diverge
- $\frac{\partial{}}{\partial w}J(w,b)$  tell you which direction you want to take your step


### Multiple linear regression

Notaions:
- n : number of featues
- $x_j$ : $j^{th}$ feature
- $\overrightarrow{x}^{(i)}$ : features of $i^{th}$ training examples ,such as $\overrightarrow{x}^{(2)}=[1,2,3,4]$ -
- $\overrightarrow{x}^{(i)}_j$ : value of features j in $i^{th}$ training example 
-  $\overrightarrow{w}=[w_1,w_2,\dots,w_n]$ 
-  $\overrightarrow{x}=[x_1,x_2,\dots,x_n]$ 

$$
f_{\overrightarrow{w},b}(\overrightarrow{x})=\overrightarrow{w}\cdot\overrightarrow{x}+b
$$

#### Cost Function

$$
J(\overrightarrow{w},b)=\frac{1}{2m}\sum_{i=1}^m(\hat y^{(i)}-y^{(i)})^2
$$

#### Gradient descent

$$
w_n=w_n-\alpha \frac{\partial{J(\overrightarrow{x},b)}}{\partial w_n}=w_n-\alpha \frac1m \sum_{i=1}^m (\hat{y}^{(i)}-y^{(i)})*x_n^{(i)}
$$

$$b=b-\alpha \frac{\partial{J(\overrightarrow{x},b)}}{\partial b}=b-\alpha \frac1m \sum_{i=1}^m (\hat{y}^{(i)}-y^{(i)})$$

### Feature Scalling

perform some transformation of  your training data,enable gradient descent to run much faster 

a very samll change to w,can have a very large impact on the estimated y and that is very large impact on the cost J

#### divide by maximum

$$
x_i=\frac{x_i}{x_{max}}
$$

#### Mean normalization

$$
x_i=\frac{x_i-\mu}{x_{max}-x_{min}}
$$
#### Z-score normalization

$$
x_i=\frac{x_i-\mu}{\sigma}
$$


### Polynomail Regression


$$
f_{\overrightarrow{w},b}(x)=w_1x+w_2x^2+w_3x^3+b
$$

do feature scalling for $x$ 


## Classification

predit categories from samll number of possible outputs


### Logistic Regression

used to solve binary classification

use sigmoid functon ,which outputs betweent 0 and 1
$$
g(z)=\frac{1}{e^{-z}}
$$

#### Decision Boundary

$$
f_{\vec w,b}(\vec x)=g(\vec w\cdot\vec x+b)=\frac{1}{e^{-(\vec w\cdot\vec x+b)}}=P(y=1|x;w,b)
$$


$$
z=\vec w \cdot \vec x+b=0
$$
#### Cost Function

squared error cost function is not a good choice


$$loss\quad  L= \begin{cases} -logf_{\vec w,b}(\vec x^{(i)} ), & y^{(i)}==1 \\ -log(1-f_{\vec w,b}(\vec x^{(i)} )) & y^{(i)}==0 \end{cases} $$

$$
L=-y^{(i)}logf_{\vec w,b}(\vec x^{(i)} )-(1-y^{(i)}log(1-f_{\vec w,b}(\vec x^{(i)}) ))
$$

$$
J(\vec w,b)=-\frac1m(y^{(i)}logf_{\vec w,b}(\vec x^{(i)} )+(1-y^{(i)})log(1-f_{\vec w,b}(\vec x^{(i)}) ))
$$

#### Gradient descent


$$
w_n=w_n-\alpha \frac{\partial{J(\overrightarrow{x},b)}}{\partial w_n}=w_n-\alpha \frac1m \sum_{i=1}^m (f_{\vec w,b}(\vec x^{(i)} )-y^{(i)})*x_n^{(i)}
$$

$$b=b-\alpha \frac{\partial{J(\overrightarrow{x},b)}}{\partial b}=b-\alpha \frac1m \sum_{i=1}^m (f_{\vec w,b}(\vec x^{(i)} )-y^{(i)})$$


## Regularization

let's you keep all of your features ,but just prevent the feature from having an overly  large effect

add a regularization term to the cost function

$$
J(\overrightarrow{w},b)=\frac{1}{2m}\sum_{i=1}^m(f_{\vec w,b}(\vec x^{(i)} )-y^{(i)})^2+\frac{\lambda}{2m}\sum_{i=1}^mw_j^2
$$

$\lambda$ : regularization parameter
$\frac{\lambda}{2m}\sum_{i=1}^mw_j^2$  : regularization term

- $\lambda ->0$ : overfit
- $\lambda ->\infty$  : underfit


$$
\frac{\partial{J(\overrightarrow{x},b)}}{\partial w_n}=\frac1m \sum_{i=1}^m (f_{\vec w,b}(\vec x^{(i)} )-y^{(i)})x_n^{(i)}+\frac{\lambda}{m}w_n
$$

$$\frac{\partial{J(\overrightarrow{x},b)}}{\partial b}=\frac1m \sum_{i=1}^m (f_{\vec w,b}(\vec x^{(i)} )-y^{(i)})$$


$$\begin{aligned}
w_n&=w_n-\alpha \frac{\partial{J(\overrightarrow{x},b)}}{\partial w_n}  \\
&=w_n-\alpha (\frac1m \sum_{i=1}^m (\hat{y}^{(i)}-y^{(i)})x_n^{(i)}+\frac{\lambda}{m}w_n)\\
&=w_j(1-\alpha \frac{\lambda}{m})-\alpha\frac1m \sum_{i=1}^m (f_{\vec w,b}(\vec x^{(i)} )-y^{(i)})x_n^{(i)}
\end{aligned}$$


### underfit and overfit

### addressing overfitting

- collect more training examples -> large tarining set
- select features to include/exclude
- regularization








# unsupervised learning

find something intersting in unlabeled data,not giving the algorithm the right  answer for the example in advance

## Cluster

place thr unlabeled data into different clusters Group similar data point together

examples such as
- Google News
- DNA microarray
- Grouping customers

# A
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefinedPasted%20image%2020230708201904.png)