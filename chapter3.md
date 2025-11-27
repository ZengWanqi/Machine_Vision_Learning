参考书籍：
- [《视觉SLAM十四讲》（第1版）](https://pan.baidu.com/s/1CN4u8kRz2iuFh6VH_Ivg7A?pwd=xxxt )
- [《视觉SLAM十四讲》（第2版）](https://pan.baidu.com/s/1CN4u8kRz2iuFh6VH_Ivg7A?pwd=xxxt )

## 3.4 四元数
### 3.4.1 四元数的定义
- 四元数是一种扩展的复数，它**既是紧凑的，也没有奇异性**（关于这一点，详见书上内容），它能够表示三维空间的旋转
- 一个四元数 **q** 拥有一个实部和三个虚部：
$$
q=q_0+q_1i+q_2j+q_3k
$$
  其中$i, j, k$为四元数的三个虚部。这三个虚部满足关系式：
$$
\left\{\begin{array}{l}i^2=j^2=k^2=-1 \\ ij=k, ji=-k \\ jk=i, kj=-i \\ ki=j,ik=-j\end{array}\right.
$$
  关于上面这个关系式，一定不要理解它的几何意义，建议看完[相关b站视频](https://www.bilibili.com/video/BV1SW411y7W1?vd_source=c4c1a4bb7c0d6e9be48f54f079a0eed9)后再来学习。
- 我们常用（一个标量 + 一个向量）来表达四元数：
$$
\boldsymbol{q}=[sv],\quad s=q_0 \in \mathbb{R},\boldsymbol{v}=[q_1,q_2,q_3]^T \in \mathbb{R}^3
$$
- **实四元数**：虚部为0
- **虚四元数**：实部为0
- 三维空间中的旋转用**单位四元数来表示**
- 假设某个旋转是绕单位向量 $\boldsymbol{n}=[n_x,n_y,n_z]^T$ 进行了 $\theta$ 角的旋转，这个旋转的四元数形式为：
$$
\boldsymbol{q}=q_0+\boldsymbol{q_1}+\boldsymbol{q_2}+\boldsymbol{q_3}=\begin{bmatrix} \cos \frac{\theta}{2},n_{x} \sin \frac{\theta}{2}, n_{y} \sin \frac{\theta}{2}, n_{z} \sin \frac{\theta}{2}\end{bmatrix}^T
$$
  这里可能会有疑惑，明明说是旋转 $\theta$ 角，为什么感觉只旋转了一半？实则不然，这样定义的四元数 **q** 可以满足**单位四元数**的要求，即模长为1
- 反之，从单位四元数可以计算对应**旋转轴与夹角**：
$$
\left\{\begin{array}{l}\theta=2 \arccos q_{0} \\ {\left[n_{x}, n_{y}, n_{z}\right]^{T}=\left[q_{1}, q_{2}, q_{3}\right]^{T} / \sin \frac{\theta}{2}}\end{array}\right.
$$
- 任意的旋转都可以由两个互为相反数的四元数表示，换句话说，**互为相反数的单位四元数会产生完全相同的旋转效果**（不好理解，可以先往下学，再回头来看就知道了）
### 3.4.2 四元数的运算
1. 加减法
2. 乘法
    - **四元数不满足乘法交换律**
3. 共轭
    - **四元数的共轭就是把虚部取为相反数**：
    - 四元数的共轭与自身相乘，会得到一个**实四元数**，其实部为模长的平方
$$
\left\{\begin{array}{l}\boldsymbol{q}=q_0+q_1i+q_2j+q_3k \\ \boldsymbol{q^*}=q_0-q_1i-q_2j-q_3k \end{array}\right.
$$
4. 模长
    - 四元数乘积的模 = 四元数模的乘积 \quad 这确保了**单位四元数相乘后仍是单位四元数**
5. 逆
    - 四元数的逆的定义式为
$$
\boldsymbol{q}^{-1}=\boldsymbol{q}^{*} /\|\boldsymbol{q}\|^{2}
$$
6. 数乘与点乘
### 3.4.3 用四元数表示旋转
先进行如下假设：
- 三维空间中有一点 $\boldsymbol{p}=[x, y, z]$
- 有一个由轴角为 $\boldsymbol{n}, \theta$ 指定的旋转
- 旋转过后的点为 $\boldsymbol{p'}$
用四元数来表示这个旋转的步骤如下：
1. 首先，把三维空间中的点 $$\boldsymbol{p}$ 用**虚四元数**来表示：
$$
\boldsymbol{p}=[0, x, y, z]=[0, \boldsymbol{v}]
$$
2. 然后，用四元数 $\boldsymbol{q}$ 表示这个旋转：
$$
\boldsymbol{q}=\left[\cos \frac{\theta}{2}, \boldsymbol{n} \sin \frac{\theta}{2}\right]
$$
3. 最后，旋转后的点 $\boldsymbol{q'}$ 即可表示为这样的乘积：
$$
\boldsymbol{p}^{\prime}=\boldsymbol{q} \boldsymbol{p} \boldsymbol{q}^{-1}
$$
  省略证明，计算结果的实部为0，故为虚四元数。其虚部的三个分量表示旋转后 3D 点的坐标。
### 3.4.4 四元数到旋转矩阵的转换
- 设单位四元数 $\boldsymbol{q}=q_{0}+q_{1} i+q_{2} j+q_{3} k$ ,则对应的旋转矩阵 $\boldsymbol{R}$ 为：
$$
\boldsymbol{R} = \begin{bmatrix}
1 - 2q_2^2 - 2q_3^2 & 2q_1q_2 + 2q_0q_3 & 2q_1q_3 - 2q_0q_2 \\
2q_1q_2 - 2q_0q_3 & 1 - 2q_1^2 - 2q_3^2 & 2q_2q_3 + 2q_0q_1 \\
2q_1q_3 + 2q_0q_2 & 2q_2q_3 - 2q_0q_1 & 1 - 2q_1^2 - 2q_2^2
\end{bmatrix}
$$
- 设旋转矩阵 $\boldsymbol{R} = \{m_{ij}\}, \, i,j \in [1,2,3]$ ,则对应的四元数 $\boldsymbol{q}$ 由下式给出：
$$
q_0 = \frac{\sqrt{\operatorname{tr}(R) + 1}}{2},\, q_1 = \frac{m_{23} - m_{32}}{4q_0},\, q_2 = \frac{m_{31} - m_{13}}{4q_0},\, q_3 = \frac{m_{12} - m_{21}}{4q_0}
$$

### B站视频学习：四元数如何控制物体旋转
下面小节为学习 b站 视频 [四元数如何控制物体旋转？](https://www.bilibili.com/video/BV14t421h7M4?vd_source=c4c1a4bb7c0d6e9be48f54f079a0eed9) 所做的笔记：
- 叉乘
  - **叉乘**是描述**三维向量旋转**的语言
  -  用单位向量 **u** 去叉乘，可以把垂直 **u** 的任意向量 **v** 绕旋转 $90^\circ$ 
  - **叉乘**满足负交换律，-1 表示旋转 $180^\circ$，右乘表示顺时针旋转 $90^\circ$
- 旋转
  - 相同的旋转轴角，若旋转顺序不同，则旋转结果也可能不同
  - 旋转可以合并：绕不同轴的多次旋转，可以合并为绕某个轴的一次旋转
  - 不仅欧拉角的**偏航 - 俯仰 - 滚转**能表示物体的姿态，**单个轴角**也能表示物体的姿态
- 四元数
  - 凡是满足平方得 -1 的数都是虚数单位i，**单位球面上的四元数都是虚数单位**，凡是复数 i 满足的性质，这些四元数都能够满足
  - **单位四元数的乘法表示四维空间的双旋转**
  - i 左乘自己，则是在复数平面内旋转 $90^\circ$ ，跑出三维空间，进入实数轴
  - 单位球面上的任意四元数 **v** 都与虚数 i 等价，我们总能给 **v** 确定两个平面：
    1. 以 **v** 为法线的过原点的平面1
    2.  **v** 与实数轴张成的复平面2
  - 四维双旋转就在上述的两个平面内发生：
    1. **v** 左乘可以把三维平面1内的向量逆时针旋转 $90^\circ$ ，右乘则是顺时针旋转 $90^\circ$ 
    2. 由于复数乘法具有交换律，因此在复平面2内无论左乘右乘，都是逆时针旋转 $90^\circ$
  - **注意**，对于向量 **v** 而言，无论长度，只要沿着 **v** 轴都会在复平面旋转 $90^\circ$ ，跑出三维空间
- 四元数共轭
  - 四元数共轭 $q^*$ 表示一次反向旋转操作，$q^*$ 也就是 **q**的 -1 次方
- 总结
这个视频讲的一般吧，推荐指数三颗星。
### B站视频学习：四元数的可视化
下面小节为学习 b站 视频 [四元数的可视化](https://www.bilibili.com/video/BV1SW411y7W1?vd_source=c4c1a4bb7c0d6e9be48f54f079a0eed9) 所做的笔记：
- 正如复数是实数的二维延伸，**四元数则是复数的四维延伸**，四元数在描述三维旋转方面是非常实用的
- 四元数可以优雅地描述并计算三维旋转
- 我们需要给复数加上的不是一个维度，而是两个额外的遐想的维度，即一共三个虚维度来描述空间，而实数则在第四个维度，垂直与全部三个虚数轴。
<center>
<img src=".\images\figure1.png" alt="图" style="zoom:67%;" />
</center>
- 总结
这个视频讲的非常好，对于理解四元数的意义非常有帮助，就是有点难懂，至少要看两遍，推荐指数五颗星
## 3.5 相似、仿射、射影变换

参考书籍：[《视觉SLAM十四讲》]( https://pan.baidu.com/s/1lcbQMu1Sz_bO0YTmHbhx1w?pwd=xxxt )
1. 相似变换
相似变换允许物体进行均匀地缩放，其矩阵表示为：
$$
\boldsymbol{T}_S = \begin{bmatrix} s\boldsymbol{R} & \boldsymbol{t} \\ \mathbf{0}^T & 1 \end{bmatrix}
$$
2. 仿射变换
仿射变换的矩阵表示为：
$$
\boldsymbol{T}_A = \begin{bmatrix} \boldsymbol{A} & \boldsymbol{t} \\ \mathbf{0}^T & 1 \end{bmatrix}
$$
与欧式变换不同，仿射变换只要求 $\boldsymbol{A}$ 是一个可逆矩阵，而不必是正交矩阵
3. 射影变换
射影变换是最一般的变换，其矩阵表示为：
$$
\boldsymbol{T}_P = \begin{bmatrix} \boldsymbol{A} & \boldsymbol{t} \\ \boldsymbol{a}^T & v \end{bmatrix}
$$
从真实世界到相机照片的变换可以看成一个射影变换。
## 3.6 实践：Eigen 几何模块
尝试在 Eigen 中使用四元数、欧拉角和旋转矩阵
首先要在终端执行下面命令来克隆远程仓库
`git clone https://github.com/gaoxiang12/slambook`
