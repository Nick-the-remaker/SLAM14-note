# 相机与图像
## 相机模型
### 1.针孔相机模型
![](/image/chapter5/pinhole.png)
+ 针孔模型是最简单的相机模型，在这个模型中假设三维世界中的物体通过针孔投影到了一个二维平面。通过示意图的相似三角形公式可得知，远处物体的大小可以通过一个相机参数——**焦距** (focal length)来描述。
$$-x = f \frac{X}{Z}$$
而由于相机硬件会将图像正过来，所以上式变成了
$$\frac{x}{f} = \frac{X}{Z}$$
+ 在实际相机中，由于安装误差，相机芯片的中心会和光轴有一些偏移，所以我们引入**Cx**和**Cy**两个参数，假设蜡烛的物理世界坐标为(X,Y,Z)，在相机成像平面投影后的坐标为(Xc,Yc)，则：
$$u=fx(\frac{X}{Z})+c_x \space, v=fy(\frac{Y}{Z})+c_y$$
> 参考《Learning OpenCV》，对焦距$f_x$和$f_y$是这样描述的：
焦距$f_x$实际上是透镜物理焦距长度F(mm)与成像仪每个单元尺寸$s_x$(pixel/mm)的乘积，$f_y$同理。所以$f_x,f_y,c_x,c_y$的单位都是像素。
mm只是一个物理单位，也可以是米或者其他，关键在于$s_x$把物理单位转换为像素单位。
+ 相机内参
我们使用齐次坐标（将一个原本是n维的向量用一个n+1维向量来表示）来描述图像平面上的点。
$$\begin {bmatrix} u \\ v \\ 1 \end{bmatrix} = \frac{1}{Z} \begin {bmatrix} f_x & 0 & c_x\\ 0 & f_y & c_y \\ 0 & 0 & 1 \end{bmatrix} \begin {bmatrix} X \\ Y \\ Z \end{bmatrix} \stackrel{def}{=} \frac{1}{Z}KP$$
$$Z\begin {bmatrix} u \\ v \\ 1 \end{bmatrix} = \begin {bmatrix} f_x & 0 & c_x\\ 0 & f_y & c_y \\ 0 & 0 & 1 \end{bmatrix} \begin {bmatrix} X \\ Y \\ Z \end{bmatrix} \stackrel{def}{=} KP$$
这里的K便是相机内参

+ 接下来是从世界坐标系到图像坐标系的转变过程
**世界坐标系到相机坐标系**：我们指定一个原点P为世界坐标系的原点，这个P可以是机器人的机构，也可以是被识别的物体。
我们将P点从世界系下的坐标转换到相机坐标系得：
$$ZP_{uv}=Z\begin {bmatrix} u \\ v \\ 1 \end{bmatrix}=K(RP_W+t)=KTP_W$$
这里可以看到相比上式的变化是：用旋转矩阵R和平移向量t来描述P，即：
$$P_C=RP_W+t$$
写成矩阵形式就是：
$$\begin {bmatrix} P_C \\ 1  \end{bmatrix}=\begin {bmatrix} R&T \\ 0^T&1  \end{bmatrix}\begin {bmatrix} P_W \\ 1 \end{bmatrix}$$
其中，相机的位姿R，t又称相机的外参数，也是SLAM中待估计的目标，代表机器人的轨迹。
这时的$P_C$仍有X,Y,Z三个量，将它们投影到Z=1的归一化平面上，得到$P_C$的归一化相机坐标 $\begin {bmatrix} X/Z && Y/Z &&1  \end{bmatrix}^T$，左乘内参K即可得到像素坐标。

总结可得下式：
$$Z_C \begin {bmatrix} u \\ v \\ 1  \end{bmatrix} = \begin {bmatrix} f_x & 0 & c_x & 0\\ 0 & f_y & c_y & 0 \\ 0 & 0 & 1 & 0\end{bmatrix} \begin {bmatrix} R&T \\ 0^T&1  \end{bmatrix} \begin {bmatrix} X_W \\ Y_W \\ Z_W \\ 1  \end{bmatrix}$$

### 2.畸变模型
+ 由于没有完美的透镜，也没有完美的装配，所以透镜的加入，引入了成像上的畸变，这里主要描述两种畸变模型，径向畸变和切向畸变。
![](/image//chapter5/distorted.png)
+ **径向畸变**：由透镜形状引起，光线越远离透镜中心越弯曲。径向畸变可以用 r = 0（光学中心）位置周围的泰勒级数来描述。对于普通的相机，通常使用前两项k1，k2来描述，鱼眼相机可以使用第三项k3。成像仪某点的径向位置可以用下式进行调节：
$$x_{distorted} = x(1+k_1r^2+k_2r^4+k_3r^6) \\ y_{distorted} = y(1+k_1r^2+k_2r^4+k_3r^6)$$
+ **切向畸变**：由透镜本身和图像不平行产生的，可以用两个额外参数P1，P2来描述：
$$x_{distorted} = x + 2p_1xy + p_2(r^2+2x^2) \\ y_{distorted} = y + p_1(r^2+2y^2) + 2p_2xy$$
+ 将五个畸变系数综合起来可得：
$$f(x)=
\begin{cases}
x_{distorted} = x(1+k_1r^2+k_2r^4+k_3r^6) + 2p_1xy + p_2(r^2+2x^2)\\
y_{distorted} = y(1+k_1r^2+k_2r^4+k_3r^6) + p_1(r^2+2y^2) + 2p_2xy
\end{cases} $$
+ 将畸变后的点通过内参矩阵投影到像素平面，即可得到该点在图像上的正确位置。
$$\begin{cases}
u = f_x x_{distorted} + c_x\\
v = f_y y_{distorted} + c_y
\end{cases}$$

### 3.双目相机模型
+ 假设我们由两套光轴严格平行，距离一定，焦距相同，水平放置的两台相机，其相机光圈中心的距离称为双目相机的**基线**。
![](/image/chapter5/shuangmu.png)
+ 成像模型：$\Delta PP_LP_R$与$\Delta PPO_LO_R$相似，其中，P是空间中一点，$P_L$和$P_R$分别为其在左右相机中成的像，$O_L,O_R$是两个相机的光圈中心，b是基线。
> 注：右相机的坐标系中$u_R$是负数，所以距离为$-u_R$
+ 综上可得关系式：
$$\frac{z-f}{z}=\frac{b-u_L+u_R}{b}$$
整理得：
$$z=\frac{fb}{d} \space,d\stackrel{def}{=}u_L-u_R$$
其中，d为左右图的横坐标之差，称为视差。由公式可看出，深度z的测量和fb与d有关，d越大，基线越长，能测量的深度就越远，而d最小为一个像素值，所以双目相机理论上能测量的最大深度由fb决定。
### 4.RGB-D相机模型
+ RGB-D相机可主要分为两大类，ToF类和结构光类。
+ ToF类：Time-of-Flight 飞行时间，通过探测光脉冲的飞行往返时间来获取深度信息。
+ 结构光类：Structured-light，通过近红外激光器，投射出具有一定结构特征的激光散斑，这样的散斑照射到物体上会因物体形状产生相应的畸变，运算单元会将这种结构的变化换算成深度信息。
## 图像
+ 我们使用Open CV进行图像数据处理，使用cv::Mat 数据结构来操作图像，基本构成：行数（高），列数（宽），通道数（高）以及数据类型。
+ 灰度图像的存储和操作是化为同行同列的二维数组，其中每一个坐标都代表相关的灰度值，灰度值共有256级，0到255是由黑色到白色的逐级过渡。
+ 彩色图像中，使用红绿蓝三原色来表达不同的颜色，即B、G、R三个分量的占比，操作彩色图片时使用BGR三个通道(channel)，每个通道都由8位整数表示。
