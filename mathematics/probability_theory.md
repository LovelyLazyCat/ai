# 概率论笔记

概率论是机器学习中用得非常多的一门数学学科.可以这么说:机器学习实际上就是在做统计分析.这个笔记记录了在机器学习中用得到的一些概率论基本知识.

## 随机变量

**随机变量**指的是可以随机地取不同值的变量.它可以是连续或者离散的.

例如,对于"抛一枚硬币"这个事件,"硬币哪个面朝上"就是一个随机变量,它只有两种取值:"正面朝上"和"反面朝上".显然,这是一个离散的随机变量.

## 概率分布

**概率分布**是用来描述随机变量或一簇随机变量在每个状态的可能性大小.对于离散和连续的随机变量,我们分别通过**概率质量函数**和**概率密度函数**来描述.

### 概率质量函数

概率质量函数是一群离散的取值,它表示了随机变量取某个值的概率.通常用$P(\mathbf{x})$来表示.用$\mathbf{x}\sim P$表示随机变量遵守一个分布.

假如随机变量有多个,则组成**联合概率分布**,例如$P(\mathbf{x}=x,\mathbf{y}=y)$表示二者同时发生的概率,简写为$P(x,y)$.

注意对于概率质量函数,有如下限制:

- $P$的定义域必须是$\mathbf{x}$所有可能状态的集合.
- $\forall x\in\mathbf{x},0\le P(x)\le1$.
- $\sum_{x\in \mathbf{x}}P(x)=1$.

### 概率密度函数

概率密度函数用于研究连续型随机变量,常用$p(x)$表示,概率密度函数有以下性质:

- $p$的定义域是$\mathbf{x}$所有可能状态的集合.
- $\forall x\in \mathbf{x},p(x)\ge0$注意不要求$p(x)\le1$.
- $\int p(x)\mathrm{d}x=1$

注意,概率密度的值$p(x)$并不是概率,概率是通过函数包括的面积给出的,需要通过积分去计算.

## 边缘概率

对于联合概率分布,有边缘概率的概念.指的是想了解多个随机变量中特定的某个变量的分布.

概率质量函数的边缘概率计算公式为:

$$\forall x\in \mathbf{x},P(x=\mathbf{x})=\sum_yP(\mathbf{x}=x,\mathbf{y}=y)$$

对于概率密度函数,计算公式如下:

$$p(x)=\int p(x,y)\mathrm{d}x$$

## 条件概率

在很多情况下,我们希望求出在给定其它事件发生的情况下发生某事件的概率.这叫做**条件概率**.条件概率通过下面的公式计算:

$$P(\mathbf{y}=y|\mathbf{x}=x)=\frac{P(\mathbf{y}=y,\mathbf{x}=x)}{P(\mathbf{x}=x)}$$

注意只有在$P(\mathbf{x}=x)>0$的时候,条件概率才是有意义的.

对于很多随机变量的条件概率,具有链式法则:

$$P(x^{(1)},\dots,x^{(n)})=P(x^{(1)})\prod^n_{i=2}P(x^{(i)}|x^{(1)},\dots,x^{(i-1)})$$

## 独立性

对两个随机变量,若它们的概率分布可以表示为两个因子的乘积形式,则这两个随机变量是**相互独立的**,即:

$$\forall x\in\mathbf{x},y\in\mathbf{y},p(\mathbf{x}=x,\mathbf{y}=y)=p(\mathbf{x}=x)p(\mathbf{y}=y)$$

如果关于另外一个随机变量的条件变量可以写成上述形式,则称为**条件独立**:

$$\forall x\in\mathbf{x},y\in\mathbf{y},z\in\mathbf{z},p(\mathbf{x}=x,\mathbf{y}=y|\mathbf{z}=z)=p(\mathbf{x}=x|\mathbf{z}=z)p(\mathbf{y}=y|\mathbf{z}=z)$$

可以用简化形式$\mathbf{x}\bot\mathbf{y}$表示独立,$\mathbf{x}\bot\mathbf{y}|\mathbf{z}$表示条件独立.

## 期望,方差和协方差

函数$f(x)$关于某个分布$P(\mathbf{x})$的期望记为$\mathbb{E}_{\mathbf{x}\sim P}$.

对于离散随机变量,期望的计算公式为:

$$\mathbb{E}_{\mathbf{x}\sim P} [ f(x)]=\sum_xP(x)f(x)$$

对于连续型随机变量,通过求积分计算:

$$\mathbb{E}_{\mathbf{x}\sim P} [ f(x)]=\int p(x)f(x)\mathrm{d}x$$

期望可以理解为均值.例如抛一枚硬币,我们知道结果是均匀分布.假设正面朝上得5分,反面朝上得2分,则可以算得得分的期望是:$5\times0.5+2\times0.5=3.5$,也就是说得分的均值为3.5分.

**方差**衡量我们对$x$依据它的概率分布采样时,随机变量$\mathbf{x}$的函数值会产生多大的差异:

$$\mathbf{V}ar(f(x))=\mathbb{E} [ (f(x)-\mathbb{E} [ f(x)])^2]$$

当方差比较小的时候,$f(x)$形成的簇比较接近它们的期望值.方差的平方根被称为**标准差**.

**协方差**在某种意义上给出了两个变量线性相关性的强度:

$$\mathbf{C}ov(f(x),g(y))=\mathbb{E} [ (f(x)-\mathbb{E} [ f(x)])(g(y)-\mathbb{E} [ g(y)])]$$

当协方差的绝对值比较大的时候,意味着两个变量的相关性较大.协方差为负数,则两个变量是负相关的(一个变量增大,另外一个变量减小).否则,是正相关的.

如果两个变量相互独立,则它们的协方差为0.但是协方差为0的两个变量并不一定相互独立.这只能保证两个变量没有线性关系,不保证它们没有非线性关系,而相互独立要求两个变量没有任何关系.

一个向量$x\in \mathbb{R}^n$的**协方差矩阵**是一个$n\times n$的矩阵,满足:

$$\mathbf{C}ov(x)_{i,j}=\mathbf{C}ov(x_i, x_j)$$

协方差矩阵的对角元素是方差:

$$\mathbf{C}ov(x_i,x_i)=\mathbf{V}ar(x_i)$$

## 常见的概率分布

下面的一些常见分布在机器学习中经常用到:

### Bernoulli分布

一个二值分布,有:

$$P(\mathbf{x}=1)=\phi$$

$$P(\mathbf{x}=0)=1-\phi$$

期望和方差是:

$$\mathbb{E}=\phi$$

$$\mathbf{V}=\phi(1-\phi)$$

### Multinoulli

这是一个多值分布,是Bernoulli的多值形式.也是一个离散随机分布.

### 高斯分布

一个非常重要的分布,也叫**正态分布**,这是一个连续的分布,在二维情况下,记为:

$$\mathcal{N}(x;\mu,\sigma^2)=\sqrt{\frac{1}{2\pi\sigma^2}}\exp(-\frac{1}{2\sigma^2}(x-\mu)^2)$$

$\frac{1}{2\sigma^2}$不利于在机器学习的时候进行求值,因此引入**精度**$\beta=\frac{1}{\sigma}$.这可以利于对密度函数的求导等操作.

正态分布由两个参数控制,$\mu\in\mathbb{R}$和$\sigma\in(0,\infty)$.$\mu$给出了中心峰值的坐标,因此高斯分布的均值$\mathbb{E}=\mu$.分布的标准差为$\sigma$,方差为$\sigma^2$.

根据**中心极限定理**,很多独立随机变量的和近似服从高斯分布.所以在机器学习中,我们在初始化参数时常常使用高斯分布.

高斯分布可以推广到$\mathbb{R}^n$空间,这时均值$\mu$变成了一个向量,而原来的方差变为了多个变量的协方差矩阵$\Sigma$:

$$\mathcal{N}(x;\mu,\Sigma)=\sqrt{\frac{1}{(2\pi)^2\det(\Sigma)}}\exp(-\frac{1}{2}(x-\mu)^T\Sigma^{-1}(x-\mu))$$

这里的$\Sigma$是一个正定对称矩阵,因此它是非奇异的,可以求逆操作,并且det不为0.

同样,这里也有**精度矩阵**,它就是$\Sigma$的逆矩阵.

### 指数分布

指数分布的定义为:

$$p(x;\lambda)=\lambda\mathbf{1}_{x\ge0}\exp(-\lambda x)$$

其中,$\mathbf{1}_{x\ge0}$表示指示函数,它用于让$x$取负值时概率为0.

一个联系紧密的概率分布是Laplace分布,它允许我们在任意一点$\mu$处设置概率质量的峰值:

$$\mathbf{Laplace}(x;\mu,\gamma)=\frac{1}{2\gamma}\exp(-\frac{|x-\mu|}{\gamma})$$

## 香农熵

信息论的基本想法是要需要一个能够量化信息的函数,这个函数要满足下面的性质:

- 发生概率较高的信息的信息量比较少.极端情况下,确保能发生的事件的信息量为0.
- 发生概率较低的事件的信息量比较大.
- 独立事件应该具有增量的信息.例如$n$个独立事件发生的信息量应该是单独独立事件的信息量的$n$倍.

为了满足上述性质,定义**自信息**,为:

$$I(x)=-\log P(x)$$

注意$\log$表示自然对数.$I(x)$的单位是**奈特**.若使用底数为2来表示$\log$,则单位是**比特**,或**香农**.

自信息只处理单个信息的信息量,如果要求整个概率分布的不确定性总量进行量化,则需要**香农熵**:

$$H(x)=\mathbb{E}_{\mathbf{x}\sim P} [ I(x)]=-\mathbb{E} _{\mathbf{x}\sim P} [ \log P(x)]$$

对于一个分布的香农熵指的是遵循这个分布的事件产生的自信息的期望.

那些比较确定的分布的香农熵较低,而不确定的分布的香农熵比较高.例如,分布$[ 0.5,0.5]$的香农熵为

$$H(x)=-(0.5\times\log(0.5)+0.5\times\log(0.5))\approx0.7$$

而对于分布$[ 0.8,0.2]$的香农熵为

$$H(x)=-(0.8\times \log(0.8)+0.2\times\log(0.2))\approx0.5$$

很明显,同样是两个事件,第一个分布的不确定性比第二个分布不确定性要高.由此我们可知,均匀分布的香农熵是最大的.

## KL散度和交叉熵

如果同一个随机变量$\mathbf{x}$有两个单独的概率分布$P(\mathbf{x})$和$Q(\mathbf{x})$,则可以使用**KL散度**来衡量这两个分布的差异:

$$D_{KL}(P||Q)=\mathbb{E}_{\mathbf{x}\sim P} [ \log P(x)-\log Q(x)]$$

KL散度是非负的,KL散度越小,则两个分布越相似.KL散度为0当且仅当$P$和$Q$在离散型变量的情况下是完全相同的分布.

KL散度经常用于衡量两个分布之间的距离,但是它并不是对称的,也就是说对于某些$P$和$Q$,$D_{KL}(Q||P)\ne D_{KL}(P||Q)$.

一个和KL散度密切联系的量是**交叉熵**,它可以由香农熵和KL散度推来:

$$H(P,Q)=H(P)+D_{KL}(P||Q)$$

它的计算公式和KL散度很像,但是少了左边一项:

$$H(P,Q)=-\mathbb{E}_{\mathbf{x}\sim P}\log Q(x)$$

最小化交叉熵相当于最小化KL散度.

一个简单的例子,第一个分布是$P=[ 0.3,0.2,0.5]$,需要计算交叉熵的两个分布是$Q_1= [ 0.2,0.3,0.5]$,$Q_2=[ 0.5,0.3,0.2]$.

则有:

$$H(P,Q_1)=-(0.3\times\log0.2+0.2\times\log0.3+0.5\times\log0.5)\approx1.07$$

$$H(P,Q_2)=-(0.3\times\log0.5+0.2\times\log0.3+0.5\times\log0.2)\approx1.25$$

通过直觉都可以感觉到,$Q_1$比$Q_2$更接近$P$.计算的结果也是这样,$Q_1$的交叉熵更小.

## 估计

这里实际上进入数理统计的知识范畴了.不过这些知识和概率论联系紧密,我还是放在一起.

可以说,估计就是机器学习在做的事情,它试图通过一些方法来估计出模型中的参数,使模型可以拟合真实数据.

### 点估计

点估计实际上就是对参数$\theta$的估计.我们一般将估计值记为$\hat{\theta}$.

假设$\lbrace x^{(1)},\dots,x^{(m)}\rbrace$是m个独立同分布的数据点,则**点估计**应该为:

$$\hat{\theta}=g(x^{(1)},\dots,x^{(m)})$$

这里并不明确地指定$g$,也不要求$\hat{\theta}$接近真实的$\theta$.总之,点估计是一种非常灵活的方法,对于一个参数的点估计值有任意种计算方法(只要它是随机变量的函数).

在统计学中,我们要估计某个分布的参数,一般先对这个分布的数据进行采样,使用这些样本点来估计总体的参数.由于样本是随机采样的,因此即使使用同一种$g$,$\hat{\theta}$也是随机的,它是一个随机变量.

### 偏差

估计的偏差被描述为:

$$\mathbf{bias}(\hat{\theta}_m)=\mathbb{E}(\hat{\theta}_m)-\theta$$

之前说过,$\hat{\theta}$是一个随机变量,因此能对它求期望,这里的期望是在所有数据(从随机变量采样得到的)上的.

如果bias为0,则该点估计是**无偏**的,否则,则是**有偏的**.

下面举个非常经典的例子,加入我们有一个服从高斯分布的数据集$\lbrace x^{(1)},\dots,x^{(m)}\rbrace\sim\mathcal{N}(x^{(i)};\mu,\sigma^2)$,我们现在要估计参数$\mu$和$\sigma$.

假设我们使用**样本均值**作为$\mu$的点估计的函数,即:

$$\hat{\mu}_m=\frac{1}{m}\sum^m _{i=1}x^{(i)}$$

现在计算偏差:

$$\mathbf{bias}(\hat{\mu}_m)=\mathbb{E} [ \hat{\mu}_m]-\mu$$

$$\qquad\qquad\qquad\quad\ =\mathbb{E} [ \frac{1}{m}\sum^m _{i=1}x^{(i)}]-\mu$$

$$\qquad\qquad\qquad=(\frac{1}{m}\sum^m_{i=1}\mu)-\mu$$

$$\qquad\qquad\qquad=(\frac{1}{m}\sum^{m}_{i=1}\mu)-\mu$$

$$\qquad\qquad=\mu-\mu=0$$

我们发现,样本均值是高斯均值参数的无偏估计量.

现在,使用**样本方差**作为$\sigma^2$的估计值,即:

$$\hat{\sigma}_m^2=\frac{1}{m}\sum^m _{i=1}(x^{(i)}-\hat{\mu}_m)^2$$

则需要计算偏差:

$$\mathbf{bias}(\sigma^2_m)=\mathbb{E} [ \hat{\sigma}^2_m]-\sigma^2$$

估计项$\mathbb{E} [ \hat{\sigma}^2_m]$,我们有:

$$\mathbb{E} [ \hat{\sigma}^2_m]=\mathbb{E} [ \frac{1}{m}\sum^m_{i=1}(x^{(i)}-\hat{\mu}_m)^2]=\frac{m-1}{m}\sigma^2$$

则求得偏差为:

$$\mathbf{bias}(\sigma^2_m)=-\frac{\sigma^2}{m}$$

偏差不为0,这是一个有偏估计.我们实际上还有一个对于$\sigma^2$的无偏估计:

$$\hat{\sigma}^2_m=\frac{1}{m-1}\sum^m_{i=1}(x^{(i)}-\hat{\sigma}_m)^2$$

具体过程不再演算了,总之可以得到$\mathbb{E} [ \hat{\sigma}^2_m]=\sigma^2$.

这就是为什么我们在计算方差的适合除的项一般式$m-1$了。

### 最大似然估计

之前的点估计需要我们去猜测函数,但是这些函数是怎么来的呢?我们希望有一种准则可以让我们从不同的模型得到特定的函数作为一个好的估计,这就是最大似然估计.

现在来看机器学习的一般任务:有一组$n$个样本的数据集$\mathbb{X}=\lbrace x^{(1)},\dots,x^{(n)}\rbrace$,独立地由未知的真实数据$p_{data}(\mathbf{x})$组成.

机器学习所得的模型是$p_{model}(\mathbf{x};\theta)$,是由$\theta$确定的在相同空间上的概率分布.我们的目标是估计$\theta$使$p_{model}(\mathbf{x})$和$p_{data}(\mathbf{x})$尽可能相似.

对$\theta$最大似然估计被定义为:

$$\theta_{ML}=\arg\max\limits_\theta \mathbb{E}_{\mathbf{x}\sim\hat{p} _{data}}\log p _{model}(x;\theta)$$

这实际上就是在最小化训练集上经验分布$\hat{p}_{data}$和模型分布$p _{model}$之间的交叉熵,也就是它们之间的差异.