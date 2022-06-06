# Robustness Optimization

## Library
- [DORO: Distributional and Outlier Robust Optimization]() [github](https://github.com/RuntianZ/doro)

## 研究人员
- [Rui Gao](https://faculty.mccombs.utexas.edu/rui.gao/)

## Material
- [基于数据的分布式鲁棒优化](https://zhuanlan.zhihu.com/p/58336958)
- [随机优化、鲁棒优化和分布鲁棒优化有什么联系和区别？](https://www.zhihu.com/question/327729667/answer/726873649)
- [对抗经验风险最小化]
- [Distributionally Robust Optimization 高引论文](https://zhuanlan.zhihu.com/p/338624673)

## 线性分类器
- [Robustness and regularization of support vector machines]() JMLR2009 *考虑了正则化支持向量机，并证明了与新的鲁棒优化公式完全等效*
- [Robust twin support vector machine for pattern classification]() 2013Pattern Recognition *鲁棒的双支持向量机在计算时间和分类精度都优于传统的鲁棒SVM*
- [On security and sparsity of linear classifiers for adversarial settings]() *我们利用鲁棒优化中的最新发现来研究线性分类器的正则化和安全性之间的联系*
- [Secure kernel machines against evasion attacks]() unica2016 *利用鲁棒优化和博弈论模型将潜在的对抗数据操作知识整合到学习算法中*
- [Robust Classification]() [github](https://github.com/DarrenZhang01/robust_classification)

*粗浅的说，就是在一些特殊的情况下，鲁棒优化要我们做的就是像正则化的线性分类器一样，对对抗样本的敏感程度低，从而确保模型的一般性。*

## 非线性分类器
- [Towards Deep Learning Models Resistant to Adversarial Attacks]() ICLR2018 *从鲁棒优化的角度优化了神经网络的鲁棒性* 
- [A unified gradient regularization family for adversarial examples]() Arxiv2015 *使用统一的框架，开发了一系列梯度正则化方法，可以有效惩罚损失函数的梯度*
- [Certifiable Distributional Robustness with Principled Adversarial Training]() ICLR2018 *分布式鲁棒优化*

## 鲁棒优化理论
- [Robust optimization for non-convex objectives]() NIPS2017
- [Robust optimization over multiple domains]() AAAI2019
- [Non-convex min-max optimization: Provable algorithms and applications in machine learning]()
- [Distributionally robust optimization: A review]()
- [Distributionally Robust Stochastic Optimization with Wasserstein Distance]()