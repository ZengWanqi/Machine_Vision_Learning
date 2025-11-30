# 第 4 讲  李群与李代数
在 SLAM 的位姿优化里，**李群和李代数**的核心作用是把 “带约束的位姿优化” 变成 “无约束的普通优化”，**让计算更简单**。
## 李群李代数基础
之前我们讲过，三维旋转矩阵构成了**特殊正交群** $SO(3)$，而变换矩阵构成了**特殊欧式群** $SE(3)$。它俩的定义式如下：
$$
S O(3)=\left\{\boldsymbol{R} \in \mathbb{R}^{3 \times 3} \mid \boldsymbol{R} \boldsymbol{R}^{T}=\boldsymbol{I}, \operatorname{det}(\boldsymbol{R})=1\right\}
$$
$$
S E(3)=\left\{\left.\boldsymbol{T}=\left[\begin{array}{cc}\boldsymbol{R} & \boldsymbol{t} \\ \mathbf{0}^{T} & 1\end{array}\right] \in \mathbb{R}^{4 \times 4} \right\rvert\, \boldsymbol{R} \in S O(3), \boldsymbol{t} \in \mathbb{R}^{3}\right.
$$
下面是你需要关注到的：
- 旋转矩阵和变换矩阵**对加法不封闭**
$$
\boldsymbol{R}_{1}+\boldsymbol{R}_{2} \notin S O(3)
$$
- 旋转矩阵和变换矩阵**对乘法封闭**
$$
\boldsymbol{R}_{1}+\boldsymbol{R}_{2} \notin S O(3),\quad \boldsymbol{R}_{1} \boldsymbol{R}_{2} \in S O(3)
$$
对于这种只有**一种运算的集合**，称为**群**。
### 群
- **群**是**一种集合**加上**一种运算**的代数结构。
- 若把集合记作 $A$，运算记作  $\cdot$ ，则**群**可以记作  $G=(A,\cdot)$。
- **李群**是指具有**连续**性质的群。
$SO(3)$ 和 $SE(3)$ 在实数空间上连续。你能想象出一个物体在空间中连续运动，因此 $SO(3)$ 和 $SE(3)$ 配上矩阵乘法都能构成李群。
- $SO(3)$ 和 $SE(3)$ 对于**相机姿态估计**尤为重要。
- 每个李群都有对应的李代数
### 李代数的引出
作以下假设：
- 设 $R(t)$ 为某个相机的旋转，它随时间连续变化
由正交矩阵的约束，结合导数，可以得到：
- $\dot{\boldsymbol{R}}(t) \boldsymbol{R}(t)^{T}$ 是一个**反对称矩阵**。
同时需要回顾前面的知识：**向量的内积与外积**
- 对于 $a, b \in \mathbb{R}^{3}$，内积可以写成：
$$
\boldsymbol{a} \cdot \boldsymbol{b}=\boldsymbol{a}^{T} \boldsymbol{b}=\sum_{i=1}^{3} a_{i} b_{i}=|\boldsymbol{a}||\boldsymbol{b}| \cos \langle\boldsymbol{a}, \boldsymbol{b}\rangle
$$
- 外积可以写成：
$$
\boldsymbol{a} \times \boldsymbol{b}=\left[\begin{array}{ccc}\boldsymbol{i} & \boldsymbol{j} & \boldsymbol{k} \\ a_{1} & a_{2} & a_{3} \\ b_{1} & b_{2} & b_{3}\end{array}\right]=\left[\begin{array}{c}a_{2} b_{3}-a_{3} b_{2} \\ a_{3} b_{1}-a_{1} b_{3} \\ a_{1} b_{2}-a_{2} b_{1}\end{array}\right]=\left[\begin{array}{ccc}0 & -a_{3} & a_{2} \\ a_{3} & 0 & -a_{1} \\ -a_{2} & a_{1} & 0\end{array}\right] \boldsymbol{b} \triangleq \boldsymbol{a}^{\wedge} \boldsymbol{b}
$$
重点需要复习的是，我们通过定义了一个**反对称符号 ^** ，把外积 $\boldsymbol{a} \times \boldsymbol{b}$ 写成了矩阵与向量的乘法 $\boldsymbol{a}$^$\boldsymbol{b}$。结果就是，**我们通过使用反对称符号^，将向量 $\boldsymbol{a}$ 变成了一个反对称矩阵。**
复习完毕，我们继续回到主线学习……

- 对于一个反对称矩阵，我们可以通过运算符 $\vee$ 来找到对应的向量。

$$
\boldsymbol{a}^{\wedge}=\boldsymbol{A}=\left[\begin{array}{ccc}0 & -a_{3} & a_{2} \\ a_{3} & 0 & -a_{1} \\ -a_{2} & a_{1} & 0\end{array}\right],\boldsymbol{A}^{\vee}=\boldsymbol{a}
$$
由于 $\dot{\boldsymbol{R}}(t) \boldsymbol{R}(t)^{T}$ 是分对称矩阵,因此可以找到对应的三维向量 $\phi(t)$:
$$
\dot{\boldsymbol{R}}(t) \boldsymbol{R}(t)^{T}=\boldsymbol{\phi}(t)^{\wedge}
$$
- 对**旋转矩阵求导**，只需要左乘一个 $\boldsymbol{\phi}^\wedge(t)$ 矩阵即可。
- 不难看出，$\boldsymbol{\phi}$ 反映了 $\boldsymbol{R}$ 的导数性质，称 $\boldsymbol{\phi}$ 在 $SO(3)$ 的正切空间上。
- 旋转矩阵 $\boldsymbol{R}$ 与反对称矩阵 $\boldsymbol{\phi}^\wedge$ 通过**指数关系**发生了联系
### 4.1.3 李代数的定义
每个李群都有对应的李代数，李代数描述了李群的**局部性质**。
通用的**李代数的定义**如下：
- 李代数由一个集合 $\mathbb{V}$，一个数域 $\mathbb{F}$ 和一个二元运算 $[,]$ 组成，李代数记作 $\mathfrak{g}=(\mathbb{V},\mathbb{F},[,])$。
- 其中的二元运算被称为**李括号**。
### 4.1.4 李代数 $\mathfrak{so}(3)$
- $SO(3)$ 对应的李代数是定义在 $\mathbb{R}^{3}$ 上的向量，记作 $\boldsymbol{\phi}$ 。
- $\boldsymbol{\phi}$ 可生成反对称矩阵 $\boldsymbol{\Phi}$
$$
\boldsymbol{\Phi} = \boldsymbol{\phi}^\wedge = \begin{bmatrix} 0 & -\phi_3 & \phi_2 \\ \phi_3 & 0 & -\phi_1 \\ -\phi_2 & \phi_1 & 0
\end{bmatrix} \in \mathbb{R}^{3 \times 3}
$$
在这种 “三维向量→反对称矩阵” 的转换规则下，两个向量 $\phi_1,\phi_2$ 的李括号为：
$$
\left[\boldsymbol{\phi}_{1}, \boldsymbol{\phi}_{2}\right]=\left(\boldsymbol{\Phi}_{1} \boldsymbol{\Phi}_{2}-\boldsymbol{\Phi}_{2} \boldsymbol{\Phi}_{1}\right)^{\vee}
$$
对于上面这个等式，我认为需要分成以下3步进行理解：
1. **转成矩阵**：将向量 $\phi_1、\phi_2$ 转换成 $\boldsymbol{\Phi_1}、\boldsymbol{\Phi_2}$
2. **计算矩阵的差**：计算 $\boldsymbol{\Phi_1}\boldsymbol{\Phi_2}-\boldsymbol{\Phi_2}\boldsymbol{\Phi_1}$
    - 我们知道矩阵乘法不满足交换律，这里本质是衡量**两个矩阵“交换乘法顺序”后的差异**——这里正好对应了李括号要表达的“两个元素的顺序差异”。
    - 假如这两个矩阵为旋转矩阵，你就更能理解，为什么旋转的先后顺序不能改变了。
3. **转成向量**
    由于李括号的结果必须属于李代数，才满足定义，因此需要通过 $^\wedge$ 从矩阵转换为向量。
    $SO(3)$ 的李代数 $\mathfrak{so}(3)$ 定义如下:
$$
\mathfrak{s} \mathfrak{o}(3)=\left\{\phi \in \mathbb{R}^{3}, \boldsymbol{\Phi}=\phi^{\wedge} \in \mathbb{R}^{3 \times 3}\right\}
$$
- $\mathfrak{so}(3)$ 的内容，就是由三维向量组成的集合，每个向量对应一个反对称矩阵，用来**表示旋转矩阵的导数**，它与 $SO(3)$的关系由**指数映射**给定：
$$
\boldsymbol{R}=\exp \left(\phi^{\wedge}\right)
$$
### 4.1.5 李代数  $\mathfrak{se}(3)$
- $SO(3)$ 的李代数 $\mathfrak{so}(3)$ 位于 $\mathbb{R}^3$ 空间中。
- $SE(3)$ 的李代数 $\mathfrak{se}(3)$ 位于 $\mathbb{R}^6$ 空间中，其定义式如下：
$$
\mathfrak{se}(3) = \left\{ \boldsymbol{\xi} = \begin{bmatrix} \boldsymbol{\rho} \\ \boldsymbol{\phi} \end{bmatrix} \in \mathbb{R}^6, \boldsymbol{\rho} \in \mathbb{R}^3, \boldsymbol{\phi} \in \mathfrak{so}(3), \boldsymbol{\xi}^\wedge = \begin{bmatrix} \boldsymbol{\phi}^\wedge & \boldsymbol{\rho} \\ \mathbf{0}^T & 0 \end{bmatrix} \in \mathbb{R}^{4 \times 4} \right\}
$$
其中，$\mathfrak{se}(3)$ 的元素记作 $\boldsymbol{\xi}$，它是一个六维向量：前三维为平移 $\boldsymbol{\rho}$，后三维为旋转 $\boldsymbol{\phi}$。
- 在 $\mathfrak{se}(3)$ 中，同样是用 $^\wedge$ 符号，将六维向量转换为四维矩阵，但这里不再表示反对称：
$$
\boldsymbol{\xi}^{\wedge}=\left[\begin{array}{ll}\phi^{\wedge} & \boldsymbol{\rho} \\ \mathbf{0}^{T} & 0\end{array}\right] \in \mathbb{R}^{4 \times 4}
$$
- $\mathfrak{se}(3)$ 有类似于 $\mathfrak{so}(3)$ 的李括号：

$$
\left[\boldsymbol{\xi}_{1}, \boldsymbol{\xi}_{2}\right]=\left(\boldsymbol{\xi}_{1}^{\wedge} \boldsymbol{\xi}_{2}^{\wedge}-\boldsymbol{\xi}_{2}^{\wedge} \boldsymbol{\xi}_{1}^{\wedge}\right)^{\vee}
$$
## 4.2 指数与对数映射
### 4.2.1 $SO(3)$ 上的指数映射
我们需要关注一个问题：$exp(\boldsymbol{\phi}^\wedge)$ 该如何计算？在李群和李代数中，这个被称为**指数映射**。
- $\mathfrak{so}(3)$ 中的元素 $\phi$ 的指数映射定义如下：
$$
\exp \left(\phi^{\wedge}\right)=\sum_{n=-n} \frac{1}{n!}\left(\phi^{\wedge}\right)^{n}
$$
- 对于三维向量 $\phi$，我们定义它的模长为 $\theta$，方向为 $\boldsymbol{a}$。（只是定义而已，不要遐想）
然后，经过书上复杂的处理……最终得到了这么一个似曾相识的式子：
$$
\exp \left(\theta \boldsymbol{a}^{\wedge}\right)=\cos \theta \boldsymbol{I}+(1-\cos \theta) \boldsymbol{a} \boldsymbol{a}^{T}+\sin \theta \boldsymbol{a}^{\wedge}
$$
回忆 3.3 旋转向量和欧拉角 小节讲的，旋转向量到旋转矩阵的变换过程由**罗德里格斯公式**给出：
$$
\boldsymbol{R}=\cos \theta \boldsymbol{I}+(1-\cos \theta) \boldsymbol{n} \boldsymbol{n}^{T}+\sin \theta \boldsymbol{n}^{\wedge}
$$
  其中，旋转轴为 $\boldsymbol{n}$，旋转角度为 $\theta$。

这下我们应该发现了，$\mathfrak{so}(3)$ 实际上是由**旋转向量**组成的空间，而**指数映射**即**罗德里格斯公式**。
- 通过指数映射，我们可以把 $\mathfrak{so}(3)$ 中的任意向量对应到位于 $SO(3)$ 中的旋转矩阵。
  - 
- 通过对数映射，我们可以把  $SO(3)$ 中的任意矩阵对应到位于 $\mathfrak{so}(3)$ 中的旋转向量。
## 4.3 李代数求导与扰动模型
### 4.3.1 BCH 公式与近似形式
我们需要明确，使用李代数的目的是为了进行优化，而在优化过程中，导数是非常重要的信息。