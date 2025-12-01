# 6 非线性优化
**SLAM 模型**由**运动方程**和**状态方程**组成。
- 运动方程中的**位姿**可以用**变换矩阵**来描述，然后用**李代数**进行优化。
- 观测方程可以由**相机成像模型**给出。
其中，**内参**是随相机固定的，而**外参**则是相机的**位姿**。

&emsp;&emsp;综上所述，你已经弄清了**经典 SLAM 模型**在视觉情况下的具体表达。然而，由于**噪声**的存在，运动方程和观测方程只能近似成立，因此，我们需要讨论**如何在有噪声的数据中进行准确的状态估计**。
&emsp;&emsp;本节将介绍基本的**无约束非线性优化**方法，同时介绍**优化库**`g2o`和`Ceres`的使用方法。

## 6.1 状态估计问题
### 6.1.1 批量状态估计和最大后验估计
&emsp;&emsp;**经典 SLAM 模型**由运动方程和观测方程构成：
$$
\left\{\begin{array}{l}\boldsymbol{x}_{k}=f\left(\boldsymbol{x}_{k-1}, \boldsymbol{u}_{k}\right)+\boldsymbol{w}_{k} \\ \boldsymbol{z}_{k, j}=h\left(\boldsymbol{y}_{j}, \boldsymbol{x}_{k}\right)+\boldsymbol{v}_{k, j}\end{array}\right.
$$
&emsp;&emsp;其中，$\boldsymbol{x}_k$ 是**相机位姿**，可以由 $\boldsymbol{T}_{k} \in \mathrm{SE}(3)$ 表示。