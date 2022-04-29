# 5.1.2 先优化再离散

我们现在转到先优化后离散的方法。这种方法是通过对理想化的连续时间模型进行微分。这样做会产生一个逆时间的微分方程，然后用数值求解。(参考文献包括\[Pon+62; Hag00; SH05; Che+18b]，但这种技术是很普遍的）。

**定理5.2.**  令$$y_0\in \mathbb{R}^d$$和$$\theta\in R^m$$. 令$$f_\theta:[0,T]\times \mathbb{R}^{d}\to \mathbb{R}^{d}$$在时间上连续，在$$y$$上一致利普希茨连续，且关于$$y$$连续可微，令$$y:[0,T]\to \mathbb{R}^{d}$$为常微分方程

$$
y(0)=y_{0}, \quad \frac{\mathrm{d} y}{\mathrm{~d} t}(t)=f_{\theta}(t, y(t))
$$

的唯一解。令 $$L=L(y(T))$$ 为某终值$$y(T)$$的标量函数。那么$$\frac{\mathrm{d} L}{\mathrm{~d} y(t)}=a_{y}(t) 和 \frac{\mathrm{d} L}{\mathrm{~d} \theta}=a_{\theta}(0)$$ ，其中 $$a_{y}:[0, T] \rightarrow \mathbb{R}^{d}$$和$$a_{\theta}:[0, T] \rightarrow \mathbb{R}^{m}$$是如下微分方程系统的解:

$$
\begin{aligned}
&a_{y}(T)=\frac{\mathrm{d} L}{\mathrm{~d} y(T)}, \quad \frac{\mathrm{d} a_{y}}{\mathrm{~d} t}(t)=-a_{y}(t)^{\top} \frac{\partial f_{\theta}}{\partial y}(t, y(t))\\
&a_{\theta}(T)=0, \quad \frac{\mathrm{d} a_{\theta}}{\mathrm{d} t}(t)=-a_{y}(t)^{\top} \frac{\partial f_{\theta}}{\partial \theta}(t, y(t)) .
\end{aligned}\tag{5.1}
$$

> **备注5.3**  方程(5.1)中的向量-矩阵积，更确切地说，是向量-雅可比积。这些可以通过自动微分有效地计算出来，见附录[A.1](../../../fu-lu-a/a.1-zi-dong-wei-fen.md)。

> **备注5.4** 请注意，第二个方程中的$$a_y(t)$$（而不是$$a_θ(t)$$）并不是一个印刷错误。矢量场与$$a_θ$$无关。这可以被利用来加速穿过神经ODE的反向传播；这是我们将在第[5.4.2.1](../../5.4-ti-shi-he-ji-qiao/5.4.2-li-yong-zi-shi-ying-bu-chang-kong-zhi-qi-de-jie-gou/)节回到的话题。

> **译者注1**  梯度和微分是对偶（转置）关系。注意按微分理解$$a_y$$和$$a_\theta$$都是行向量，那么方程应该是

$$
\begin{aligned}
&a_{y}(T)=\frac{\mathrm{d} L}{\mathrm{~d} y(T)}, \quad \frac{\mathrm{d} a_{y}}{\mathrm{~d} t}(t)=-a_{y}(t) \frac{\partial f_{\theta}}{\partial y}(t, y(t))\\
&a_{\theta}(T)=0, \quad \frac{\mathrm{d} a_{\theta}}{\mathrm{d} t}(t)=-a_{y}(t)\frac{\partial f_{\theta}}{\partial \theta}(t, y(t))
\end{aligned}
$$

这点在Neural ODE原论文中也有提到。

> **译者注2**  方程中的$$\frac{\mathrm{d} L}{\mathrm{~d} y(T)}$$准确的说是$$\left.\frac{\mathrm{d} L}{\mathrm{~d} y}\right|_{y=y(T)}$$,前者是函数，后者是数.&#x20;

> **译者注3** 通过$$y(T)=y(s)+\int_{s}^{t}f_\theta(x,y(x))dx$$可以看出来，$$y(T)$$是$$y(s)$$和参数$$\theta$$的函数。

方程（5.1）被称为连续伴随方程。

这些一起给出了一种穿过ODE解的反向传播的方法。例如，考虑图1.1。梯度$$\frac{\mathrm{d} L}{\mathrm{~d} y(T)}$$是通过通常的方式在softmax和affine层中反向传播计算的。然后将方程组（5.1）从$$t=T$$到$$t=0$$进行逆向求解，以计算参数梯度$$\frac{\mathrm{d} L}{\mathrm{~d} \theta}=a_{\theta}(0)$$。

请注意，方程（5.1）需要知道作为输入的解$$y$$。通常的方法是，先正向计算的$$y(T)$$的数值近似值，用它和原来的神经ODE一起，对方程（5.1）进行扩充。用积分符号表示，这就是

$$
y(t)=y(T)+\int_{T}^{t} f_{\theta}(s, y(s)) \mathrm{d} s
$$

这被称为 "连续伴随法"，或 "先优化后离散 "的方法。导数的计算是相对于理想化的连续时间模型而言的，然后（5.1）伴随方程本身必须被离散化。

**备注5.5**  连续伴随法通常也被简单地称为 "伴随法"，特别是在现代神经微分方程文献中。 这是一个令人遗憾的术语歧义。在一些著名的著作中，"伴随法 "被用来指代离散-优化方法\[GG06]和优化-再离散方法\[Che+18b]。此外，它有时还被用来指代其他完全不同的东西。\[HNW08，定义8.2]用它来指代数值积分步骤的逆映射，因此，例如隐式欧拉步骤是显式欧拉步骤的 "伴随"。



**译者注4**  总的来说，我们有如下算法：

1. 输入$$y(0)=y_0$$.
2. 正向求解神经ODE，得到$$y(T)$$. 中间表示不用保存，实现不依赖时间的内存效率.
3. 计算$$\frac{\mathrm{d} L}{\mathrm{~d} y(T)}$$.
4. 求解ODE系统

$$
\begin{gathered}
y(T)=y(T), \quad \frac{\mathrm{d} y}{\mathrm{~d} t}(t)=f_{\theta}(t, y(t)) \\
a_{y}(T)=\frac{\mathrm{d} L}{\mathrm{~d} y(T)}, \quad \frac{\mathrm{d} a_{y}}{\mathrm{~d} t}(t)=-a_{y}(t) \frac{\partial f_{\theta}}{\partial y}(t, y(t)) \\
a_{\theta}(T)=0, \quad \frac{\mathrm{d} a_{\theta}}{\mathrm{d} t}(t)=-a_{y}(t)\frac{\partial f_{\theta}}{\partial \theta}(t, y(t))
\end{gathered}
$$

&#x20; 得到$$[y(0),a_y(0)=\frac{\mathrm{d} L}{\mathrm{~d} y(0)},a_\theta(0)=\frac{\mathrm{d} L}{\mathrm{~d} \theta}]$$.

**译者注5**  我们常常将输入数据$$x$$投影到高维$$y(0)=NN(x)$$，此时$$\frac{\mathrm{d} L}{\mathrm{~d} y(0)}$$在反向传播时就很有用。
