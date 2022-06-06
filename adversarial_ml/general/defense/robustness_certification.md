# Robustness Certification

## Material 

## Survey
- [深度学习模型鲁棒性研究综述]() 计算机学报2022

## 精确方法
精确方法主要是基于离散优化(Discrete Optimization)理论来形式化验证神经网络中某些属性对于任何可能的输入的可行性

### 可满足性模理论 （SMT）

- [Reluplex: An Efficient SMT Solver for Verifying Deep Neural Networks]() 2017
- [Certified defenses against adversarial examples]() ICLR2018
- [Output range analysis for deep feedforward neural networks]()

### 混合整数线性规划 （MILP）

- [Deep neural networks as 0-1 mixed integer linear programs: A feasibility study]() 2017
- [Evaluating robustness of neural networks with mixed integer programming]() ICLR2019

## 近似方法

### 基于凸松弛方法

- [Provable defenses against adversarial examples via the convex outer adversarial polytope]() 2018

### 基于抽象解释的方法

- [Ai 2: Safety and robustness certification of neural networks with abstract interpretation]() S&P2018
- [Differentiable abstract interpretation for provably robust neural networks]() ICML2018

### 基于lipschitz常数的方法

- [Towards fast computation of certified robustness for relu networks]() <font color="red">Fast-Lin, Fast-Lip</font>
- [Evaluating the robustness of neural networks:An extreme value theory approach]() <font color="red">CLEVER score</font>
- [On Extensions of Clever: A Neural Network Robustness Evaluation Algorithm]()

### 基于随机平滑的方法

- [Certified Adversarial Robustness with Additive Noise]()
- [Certified Adversarial Robustness via Randomized Smoothing]()
- [Fast and Effective Robustness Certification]()

### 基于区间传播的方法

- [Formal security analysis of neural networks using symbolic intervals]() Usenix2018
- [Efficient formal safety analysis of neural networks]() NIPS2018
- [Mixtrain: Scalable training of formally robust neural networks]()
- [Efficient neural network robustness certification with general activation functions]() NIPS2018 <font color="red">CROWN</font>

### 基于概率的方法

### 基于控制论的方法


