# 李群与李代数基础
## 群
+ 直观的理解：群就是一种特殊的代数结构，这种代数结构是由一种集合加上一种运算组成，我们把集合记作A，运算记为$ \cdot$，那么群记为$G=(A,\cdot)$。
> 例如：我规定“土豆list(TODO list)”是一种代数结构，其由待办事项和添加待办、删除待办这样的运算组成，那么“土豆list”也可以和“群”作类比来理解。
+ 要求的满足条件：
    1.封闭性：$\forall a_1,a_2 \in A, $ &emsp;$a_1 \cdot a_2 \in A$.
    2.结合律：$\forall a_1,a_2,a_3 \in A,$ &emsp;$ (a_1 \cdot a_2) \cdot a_3 = a_1\cdot (a_2 \cdot a_3)$.
    3.幺元：$\exists a_0 \in A,s.t. \forall a \in A ,$ &emsp;$ a_0 \cdot a = a \cdot a_0 = a$.
    4.逆：$\forall a \in A，$ &emsp;$ \exists a ^{-1} \in A,$&emsp;$s.t.$ &emsp;$ a \cdot a^{-1} = a_0$.
> 矩阵中常见的群有特殊正交群(旋转矩阵群) $SO(n)$和特殊欧式群（n维欧式变换）$SE(n)$。
> 以三维旋转矩阵 $SO(3)$为例，旋转矩阵和乘法构成了旋转矩阵群，旋转矩阵之间的乘法满足封闭性和结合性，而幺元就是单位矩阵，$R$乘以$R^{-1}$也确实等于幺元。
+ 李群：指具有连续光滑性质的群，可以直观地以相机位姿在空间中的连续旋转来想象。

## 李代数
> 以下的推导将书中的内容细分，虽然说看上去有些多，但是也将推导过程变得更容易理解，鼓励手推一遍，这样会很有成就感～

**旋转矩阵求导**
1. 定义一个三维向量$\alpha = [a_1,a_2,a_3]^T$.
引出     $\hat{} $     符号，来表示将向量$ \alpha$变成了反对称矩阵A：$\alpha \hat{} = A = \begin{bmatrix} 0&-a_3&a_2\\ a_3 & 0& -a_1 \\ -a_2 & a_1 & 0 \end{bmatrix}$.

2. 对于任意旋转矩阵R，它会随时间连续的变化，是时间的函数，满足$R(t)R(t)^T = I$，求导得$\dot R(t) R(t) ^T = -( \dot R(t)R(t)^T)^T$.
<br>
3. $\dot R(t)R(t)^T$是一个反对称矩阵，记作$\phi(t)\hat{}$，**则$\dot R(t) = \phi(t)\hat{}R(t)$**，也就是说，对旋转矩阵求导数，只要左乘一个$\phi(t)$矩阵即可。

**旋转矩阵和反对称矩阵的关系——$R(t) = exp(\phi _0 ^ t t)$**
1. R(t)在t = 0时，规定此时旋转矩阵R(t)为$I$。
2. $\dot R(t) = \phi(t)\hat{}R(t)$，此时引入一个小概念——矩阵指数。
> 有一个简单的标量一阶线性微分方程  $\dot x(t) = ax(t)$，其中$x(t) \in R, a \in R $，且初始条件为$x(0) = x_0$，那么解为$x(t) = e^{at}x_0$。
3. 有了矩阵指数的概念，我们可以轻地得出$R(t) = e^{\phi(t_0) \hat{} t}R(t_0)$。
4. 因为$R(0) = I$，在$t_0$附近，设$\phi$保持为常数$\phi_0$，所以上式可化为$R(t) = e^{\phi_0 \hat{}t}$即$R(t) = exp(\phi_0 \hat{}t)$。

**李代数的引出**
1. 按照导数定义，使旋转矩阵在t=0附近一阶泰勒展开，得：
$R(t) \approx R(t_0)+ \dot R(t_0)(t-t_0)  = I + \phi(t_0) \hat {}(t)$.
2. 可以看到$\phi$反映了R的导数性质，故称它在SO(3)上的原点 $\phi _0$ 附近的正切空间上。这个$\phi$正是李群SO(3)对应的李代数so(3)。
3. 简单地理解就是，李代数so(3)是三维向量$\phi$的集合，李群SO(3)是旋转矩阵的集合，冥冥之中两个集合建立了联系：**李群空间的任意一个旋转矩阵R，都可以用李代数空间的一个向量的反对称矩阵指数来近似**。

**李代数的定义**
1. 每个李群都有对应的李代数，李代数描述了李群的局部性质，准确地说，是单位元附近的正切空间。
2. 李代数由一个集合V、一个数域F和一个二元运算[,]组成。如果满足以下几条性质，则称(V,F,[,])为一个李代数。
    + 封闭性：$\forall X,Y \in V,[X,Y] \in V$.
    + 双线性：$\forall X,Y,Z \in V,a,b \in F$，有
                    $[aX+bY,Z] = a[X,Z]+ b[Y,Z],[Z,aX+bY] = a[Z,X]+ b[Z,Y]$.
    + 自反性：$\forall X \in V,[X,X] = 0$.
    + 雅可比等价：$\forall X,Y,Z \in V,[X,[Y,Z]] + [Z,[X,Y]]+[Y,[Z,X]] = 0$.
3. 其中的二元运算被称为李括号，例如，三维向量$R^3$上定义的叉积x就是一种李括号。

**两种李代数so(3)和se(3)**
1. 李代数 **so(3)** 是定义在$R^3$上的向量，记作$\phi$。每个$\phi$都可以生成一个反对称矩阵：
$\Phi = \phi \hat{} =  \begin{bmatrix} 0 & -\phi _3 & \phi _2 \\ \phi _3 & 0 & -\phi _1 \\ -\phi _2 & \phi _1& 0 \end{bmatrix} \in R^{3 \times 3}$
两个向量的李括号为$[\phi _1, \phi _2] = (\Phi _1 \Phi _2 - \Phi _2 \Phi _1) ^ {\vee}$.
2. 李代数 **se(3)** 与 **so(3)** 相似，**se(3)** 位于$R^6$空间中：$se(3) = \{ \xi = \begin {bmatrix} \rho \\ \phi  \end{bmatrix} \in R^6 , \rho \in R^3 , \phi \in so(3), \xi \hat{} =  \begin {bmatrix} \phi \hat{} & \rho \\ 0 ^T & 0 \end{bmatrix} \in R^{4 \times 4} \}$
这里把每个se(3)元素记作$\xi$，它是一个六维向量，前三维为平移，记作$\rho$，后三维为旋转，记作$\phi$，实质上是so(3)元素。在se(3)中，同样使用\^符号，但是^不再表示反对称，而是表示将一个六维向量转换成四维矩阵。
se(3)的李括号为：$[\xi _1 ,\xi _2] = (\xi _1 \hat{} \xi _2 \hat{}-\xi _2 \hat{} \xi _1 \hat{}) ^ {\vee}$.

## 指数与对数映射
> 公式写麻了。。。就以so(3)为例吧

**SO(3)上的指数映射**
+ 刚才已经推导出来：$R(t) = exp(\phi _0 ^ t t)$，$exp(\phi _0 ^ t t)$显然是一个矩阵的指数，这也是为什么叫它指数映射。那么具体该怎样算呢？
    首先，任意矩阵的指数映射可以写成一个泰勒展开，其结果仍是一个矩阵，即 $exp(\phi \hat{}) = \displaystyle \sum^{ \infty}_{n = 0}{\frac{1}{n!}\phi \hat{}  \space ^ n}$.
    其次，我们规定三维向量$\phi$的模长为$\theta$，方向为a，于是有$\phi = \theta a$，为了接下来运算方便，我们提前算一下简便形式，$a \hat{} a \hat{} = aa^T - I$，$a \hat{} a \hat{} a \hat{} = -a \hat{}$.
    接下来，把指数映射展开：$exp(\phi \hat{}) = exp (\theta a \hat{}) = \displaystyle \sum^{ \infty}_{n = 0} {\frac{1}{n!}(\theta a \hat{} )^ n}$.
    (公式太多了。。直接截图了🤡️)
    ![](/image/chapter4/exp.png)
    最后这个式子：$exp(\theta a \hat{}) = cos(\theta I)+ (1 - cos(\theta))aa^T+sin(\theta)a \hat{}$ ，就是罗德里格斯公式，世界线收回了！
    
+ 反过来，用对数映射也能把SO(3)李群空间中元素映射到so(3)李代数空间中去，书中给出了对数映射的公式。即$\phi = ln(R) ^ {\vee} = (\displaystyle \sum^{ \infty}_{n = 0}{\frac{-1 ^n}{n+1}}(R-I)^{n+1}) ^ {\vee}$.
但是书中也同时提到了，可以用第三章提到的方法，利用迹的性质分别求解角和转轴，更方便快捷。
> 小推导如下～

首先放出罗德里格斯公式：$R= cos(\theta I)+ (1 - cos(\theta))nn^T+sin(\theta)n \hat{}$ .
然后我们对转角$\theta$取两边的迹（矩阵的主对角线元素之和），有
$tr(R) = cos \theta tr(I) + (1-cos \theta) tr(nn^T) + sin \theta tr (n \hat{}) \\ = 3cos \theta + (1- cos \theta) \\ = 1 + 2 cos \theta$

因此：$\theta = arccos \frac{tr(R)-1}{2}$.
关于转轴n，由于旋转轴经过旋转之后方向不变，所以向量不发生改变，即$Rn = n$.

+ 最后，书中给出了李群，李代数的定义与相互的转换关系，如图
![](/image/chapter4/big.jpeg)
se(3)的关系也在图里啦。偷个懒～

## 扰动模型

**问题引出**
李代数求导分两种：根据李代数加法来对李代数求导，但是不是很方便。第二种是对李群进行左乘或者右乘微小的扰动，然后对该扰动求导，比第一种更简单。
+ 一些小技巧：$exp((\phi + \Delta \phi) \hat{}) = exp ((J_l \Delta \phi) \hat{})exp(\phi \hat{})$
    $e^x \approx 1+x + \frac{x^2}{2!} + \frac{x^3}{3!} + ...+\frac{x^n}{n!}$
    $a \space \hat{} \space b = a \times b = -b \times a = -b\space \hat{} \space a$

**so(3)李代数求导**
![](/image/chapter4/qiudao.png)
**so(3)左扰动模型**
![](/image/chapter4/raodong.png)