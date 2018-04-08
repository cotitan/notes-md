# SVM 支持向量机
### 大致公式推导
1. 找到一个分类超平面$wx+b=0$使其到数据集的间隔最大。间隔公式如下：
    
    $$ d=\frac{|wx_i+b|}{|w|}, i=1,2...,n $$ (1)

2. 设距离分类超平面最近的点为$\hat{x_i}$，则
    
    $$ \hat{d}=\frac{|w\hat{x_i}+b|}{|w|} $$ (2)

3. 式(2)中的分子不影响最终结果，可设为1，即：

    $$ \hat{d}=\frac{1}{|w|} $$ (3)
    $$ y_i(wx_i+b) \geq 1, i=1,2,...,n$$

4. 即我们的优化目标为，最大化数据集间隔。

    $$ \max_{w,b} \frac{1}{w} $$  (4.1)
    $$ s.t. \text{\ \ \ \ \ \ \ } y_i(wx_i+b) \geq 1 $$ (4.2)

5. 等同于

    $$ \min_{w,b} \frac{1}{2}w^2 $$ (5.1)
    $$ s.t. \text{\ \ \ \ \ \ \ } y_i(wx_i+b) \geq 1$$ (5.2)

6. 即带不等式约束的优化问题，可适用拉格朗日乘子法，（拉格朗日乘子法在应用到不等式约束时，应满足kkt条件，后面介绍kkt条件）。$L$对$w,b$求偏导等于0

    $$ L(w, b, \alpha) = \frac{1}{2}w^2 - \sum_{i=1}^m\alpha_i(y_i(wx_i+b)-1) $$ (6.1)
    $$ \frac{\partial{L}}{\partial{w}} = w - \sum_{i=1}^m\alpha_iy_ix_i = 0  $$ (6.2)
    $$ \frac{\partial{L}}{\partial{b}} = -\sum_{i=1}^m\alpha_iy_i = 0 $$ (6.3)
    
    由(6.2),(6.3)可知，$w=\sum_{i=1}^m\alpha_iy_ix_i$ 且 $\sum_{i=1}^m\alpha_iy_i = 0$

7. 将(6.2, 6.3)带入(6.1)，消去变量后可得如下优化目标：其中约束(7.3)来源于拉格朗日乘子法的定义。

    $$ \max_\alpha = \sum_{i=1}^m\alpha_i - \frac{1}{2}\sum_{i=1}^m\sum_{j=1}^m\alpha_i\alpha_jy_iy_j\langle x_ix_j\rangle $$ (7.1)
    $$ s.t. \text{\ \ \ \ }\sum_i\alpha_iy_i=0 $$ (7.2)
    $$ \alpha_i\geq0, i=1,2,3...n $$ (7.3)

8. 因原来(4.1)的优化目标带有不等式约束(4.2)，故需要满足KKT条件：

    $$ \alpha_i \geq 0 $$ (8.1)
    $$ y_i(wx_i+b)-1\geq 0 $$ (8.2)
    $$ \alpha_i(y_i(wx_i+b)-1)= 0 $$ (8.3)

9. 至此，问题已经变成了关于(11)的约束优化问题，这是一个二次规划问题。其求解方法可适用SMO算法，即序列最小优化化算法。其基本思路是，先固定$\alpha_i$之外的所有参数，然后求$\alpha_i$。
    * 选取一对需更新的变量$\alpha_i,\alpha_j$
    * 固定$\alpha_i,\alpha_j$之外的参数，求解式子(11), 因为存在约束(12)，故$\alpha_j$可由$\alpha_j$表示，最终式子只剩一个$\alpha_i$，即单变量的二次优化。求出$\alpha_i$之后，根据约束(12)也能求出$\alpha_j$.

## KKT 条件

* KKT条件：对于一个带有如下等式约束和不等式约束的优化目标
    
    $$ \min_x f(x) $$
    $$ s.t. \text{\ \ \ \ \ } h_i(x)=0, \text{\ \ } i=1,2,...,m$$
    $$ g_j(x) \leq 0,\text{\ \ \ \ }, \text{\ \ \ }j=1,2,...,n$$

    其拉格朗日函数为：
    
    $$ L(x, \lambda, \mu) = f(x) + \sum_{i=1}^m\lambda_i h_i(x) + \sum_{j=1}^n\mu_j g_j(x) $$

    由**不等式约束**引入的KKT条件为:

    $$ g_j(x) \leq 0 $$
    $$ \mu_j \geq 0 $$
    $$ \mu_j(g_j(x)) = 0$$

## 软间隔与正则化
* 现实中，往往很难找到一个超平面，将不同类样本完全划分开。故引入“软间隔”的概念，允许出错。即允许有些样本不满足约束：

    $$ y_i(wx_i+b)\geq 1$$

* 解决方法是引入松弛变量$\xi_i$，引入后，优化目标变为：

    $$ \min_{w,b,\xi_i} \frac{1}{2}||w||^2 + C\sum_{i=1}^m\xi_i$$
    $$ s.t. \text{ \ \ \ }y_i(wx_i+b) \geq 1-\xi_i $$
    $$ \xi_i \geq 0, i=1,2,...,m $$
    

    其中$C$为惩罚系数，值越大，则越不容许分错，值越小，则越允许出错。而$\xi_i$本身是一个损失函数，即衡量出错时的损失是多少，通常使用hinge loss。
    
    $$ hingeloss=max(0, 1-z) $$
    $$ z=y_i(wx_i + b) $$

* 常用的损失函数，即$\xi_i$：

    $$ L_{hinge}(z) = max(0, 1-z) $$
    $$ L_{exp}(z) = exp(-z) $$
    $$ L_{log}(z) = log(1 + exp(-z)) $$

## 核函数
* 当原数据集非线性可分时，比如分类超平面是一个圆，则考虑将原始数据映射到更高维度，以变成线性可分。
* 用$\phi(x)$表示x映射到特征空间后的向量，则原始问题的对偶问题变为:

    $$ \max_\alpha \sum_{i=1}^m\alpha_i - \frac{1}{2}\sum_{i=1}^m\sum_{i=1}^n \alpha_i\alpha_j y_i y_j \phi(x_i)^T\phi(x_j)$$

* 直接计算$\phi(x_i)^T\phi(x_j)$的代价很大，故设想这一个函数：

    $$ k(x_i, x_j) = \langle\phi(x_i), \phi(x_j)\rangle = \phi(x_i)^T\phi(x_j)$$

    即$x_i$与$x_j$在特征空间的内积等于它们在原始空间中通过函数$k(\cdot,\cdot)$计算的结果，这样我们就不必计算高维空间的内积。

* 怎样能作为核函数？

    只要一个对称函数（即$k(x_i, x_j)=k(x_j, x_i)$）所对应的核矩阵（元素为$k(x_i, x_j)$）半正定，它就能作为核函数使用。

* 常用核函数
    
    * 线性核$k(x_i, x_j) = x_i^T x_j$
    * 多项式核 $k(x_i, x_j) = (x_i^Tx_j)^d$
    * 高斯核 $k(x_i, x_j) = exp(-\frac{||x_i-x_j||^2}{2\delta^2}$), $\delta$为高斯核的带宽
    * 拉普拉斯核 $k(x_i, x_j) = exp(-\frac{||x_i-x_j||}{\delta})$，$\delta$为拉普拉斯核的带宽
    * sigmoid核 $k(x_i, x_j) = tanh(\beta x_i^Tx_j + \theta)$, $\beta > 0, \theta < 0$
    
    也可通过组合得到