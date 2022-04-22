# 辛积分

时间演化微分方程有两个重要的定性性质：动力学和几何学。微分方程的动力学与它的解的最终目的地有关。当时间增加到无穷大时，解是否会趋向于一个平衡点？它将是周期性的吗？还是会表现出更多的 "奇异 "行为，比如说混沌？微分方程的几何特征通常指的是在时间上不变的特征。典型的不变量能量、角动量或正交性。

此外，在长时间积分中，讨论动力学和几何学之间的区别没有意义。在重要的情况下，有可能证明保持一个几何特征就能保证计算出正确的动力学。

有时候，使用能保留动力学和几何学的定性特征是必要的。

对于哈密顿系统，我们有两个性质希望在数值格式中得到保留：能量和辛性。



**定义**  一个辛矩阵是指一个$$2 n \times 2 n$$的矩阵$$M$$满足

$$
M^{T} \Omega M=\Omega
$$

其中$$\Omega$$是一个固定的可逆[斜对称矩阵](https://zh.wikipedia.org/wiki/%E6%96%9C%E5%B0%8D%E7%A8%B1%E7%9F%A9%E9%99%A3)；这类矩阵在适当的变化后皆能表为



$$
\Omega=\left[\begin{array}{cc}
0 & I_{n} \\
-I_{n} & 0
\end{array}\right]
$$

**定义**  一个辛函数$$\varphi: \Omega \longrightarrow \mathbb{R}^{2 d}$$，其中$$\Omega \subseteq \mathbb{R}^{2 d}$$，对任何$$y\in \Omega$$，$$\varphi_y(y)$$都是一个辛矩阵。

**定义** 一个数值格式$$y_{n+1}=\varphi_h(y_n)$$是辛的，若$$\varphi_h$$是一个辛函数。

例如



$$
\begin{aligned}
&\frac{d x}{d t}=f(t, v) \\
&\frac{d v}{d t}=g(t, x)
\end{aligned}
$$

辛欧拉格式



$$
\begin{aligned}
&v_{n+1}=v_{n}+g\left(t_{n}, x_{n}\right) \Delta t \\
&x_{n+1}=x_{n}+f\left(t_{n}, v_{n+1}\right) \Delta t
\end{aligned}
$$





