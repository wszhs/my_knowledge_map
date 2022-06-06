# Exact Optimizers

## Material
- [凸优化python代码](https://github.com/PKUFlyingPig/Standford_CVX101)
- [我的凸优化学习之路](http://deanhan.com/2018/01/17/convex/#comments)
  - 学习/复习线性代数和（少量）高等数学的知识
  - 学习[《Optimization Model》](https://zhuanlan.zhihu.com/c_1056506141189664768)
  - 学习[《凸优化》](https://github.com/PKUFlyingPig/Standford_CVX101)
- [凸优化笔记](https://zhuanlan.zhihu.com/p/194308254)


***
## Library
- [Pyomo](https://github.com/Pyomo/pyomo)
- [pao](https://github.com/or-fusion/pao) 对抗优化 [文档](https://pao.readthedocs.io/en/latest/)
- [Scipy Optimization](https://zhuanlan.zhihu.com/p/301918643)
- [scikit-opt](https://github.com/guofei9987/scikit-opt)

***

## 知识点

### 凸函数
- [什么是凸函数及如何判断一个函数是否是凸函数](https://zhuanlan.zhihu.com/p/400975901)
- [共轭函数](https://www.zhihu.com/question/268862097/answer/371323504) 原函数不是凸函数，通过构造共轭函数

### 约束求解方法
- [有约束优化的求解方法](https://zhuanlan.zhihu.com/p/427898497)
- [拉格朗日对偶性](https://zhuanlan.zhihu.com/p/38182879/)
- [如何理解拉格朗日松弛技术？](https://www.zhihu.com/question/21345731/answer/57182129)
- [如何理解拉格朗日乘子法？](https://www.zhihu.com/question/38586401/answer/105588901) 这种方法将一个有$n$个变量与$k$个约束条件的最优化问题转换为一个有$n+k$个变量的方程组的极值问题，其变量不受任何约束。
- [等式约束及不等式约束下的优化问题](https://zhuanlan.zhihu.com/p/357802348)
- [浅谈最优化问题的KKT条件](https://zhuanlan.zhihu.com/p/26514613)
- [优化理论——拉格朗日乘子法和KKT条件](https://zhuanlan.zhihu.com/p/370441258) KKT条件主要应用于优化函数存在不等式约束的情况

### 对偶问题的作用
1. 在转化为对偶问题之后，原问题中的不等式约束变为等式约束
2. 在转化为maxmin形式之后，方程可以直接对X求导了。这种情况例如梯度下降等简单算法，都可以用了
3. 转化之后可以很方便的将核函数引入，低维空间转化到高维空间
4. 线性不可分的问题可以转化为线性可分

### 梯度
- [深度学习中7种最优化算法的可视化与理解](https://mp.weixin.qq.com/s/f-g7LNm3Mlc92EToVyl9HA)
- [梯度下降的可视化解释(Momentum，AdaGrad，RMSProp，Adam)
McGL
](https://zhuanlan.zhihu.com/p/147275344)
- [10个梯度下降优化算法+备忘单](https://zhuanlan.zhihu.com/p/73962541)

### 其他
- [传说中的推土机距离基础，最优传输理论了解一下](https://zhuanlan.zhihu.com/p/45980364)
- [次模优化初探](https://blog.csdn.net/qq_40889820/article/details/116233991)


