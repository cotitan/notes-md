<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

# SVM 支持向量机
### 大致公式推导
1. 找到一个分类超平面\\(wx+b=0\\)使其到数据集的间隔最大。间隔公式如下：
    
    $$ d=\frac{|wx_i+b|}{|w|}, i=1,2...,n $$ (1)

2. 设距离分类超平面最近的点为$\hat{x_i}$，则
    
    $$ \hat{d}=\frac{|w\hat{x_i}+b|}{|w|} $$ (2)

3. 式(2)中的分子不影响最终结果，可设为1，即：

    $$ \hat{d}=\frac{1}{|w|} $$ (3)

4. 即我们的优化目标为，最大化数据集间隔。

    $$
    ew$$
