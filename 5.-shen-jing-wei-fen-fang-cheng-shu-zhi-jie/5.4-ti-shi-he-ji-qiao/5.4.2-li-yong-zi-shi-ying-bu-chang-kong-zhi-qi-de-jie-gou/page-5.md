# Page 5

为了简单起见，我们现在将集中讨论显式嵌入式Runge-Kutta方法作为一类数值ODE求解器。根据第5.3.1.2节，这些方法包括许多用于神经微分方程的典型求解器，如Heun的方法或Dormand-Prince。&#x20;

> **备注 5.26** 本节的讨论实际上通常适用于其他求解器和微分方程类型。例如，可逆Heun方法和异步跃迁方法（第5.3.2节）都与Runge-Kutta方法非常相似。

这样的求解器可以分解成两个主要部分：更新规则（由Butcher tableau\[HNW08]定义），以及用于更新步长的步长控制器。(这可能只是使用一个恒定的步长）。 更新规则通常是一个求解器中比较受关注的部分。在这里，我们将专注于如何使用或修改步长控制器，使其对我们有利。 我们首先简要说明步长是如何调整的；参考文献见\[Rac21b]，\[HNW08，第II.4节]，\[But16，第271节]。