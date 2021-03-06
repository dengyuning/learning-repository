### SVM
##### 基本概念
有监督的学习模型，通常用来进行模式判别、分类以及回归分析。

##### 主要思想
1. 针对线性可分情况进行分析，对于线性不可分的情况，通常使用非线性映射算法将低维输入空间线性不可分的样本转换为高维特征空间使其线性可分。*核函数*
2. 它基于结构风险最小化理论，在特征空间中构建最优超平面，使得学习器得到全局最优化。*损失函数*

##### 适用场景

##### 优缺点
1. SVM用的是hinge loss，对异常值不敏感，而cross entropy 跟 exponential loss 都会拼命去拟合异常值。

##### 相关问题
1. 很常见的一个Kernel就是sigmoid kernel。