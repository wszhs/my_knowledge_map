# Adversarial Attack and Defense on Graph Data

## Material
- [DeepRobust](https://github.com/DSE-MSU/DeepRobust)
- [grb](https://github.com/thudm/grb)
- [Graph Adversarial Learning Literature](https://github.com/safe-graph/graph-adversarial-learning-literature)

***
## Introduction
In graph machine learning, adversarial robustness refers to the ability of GML models to maintain their performance under potential adversarial attacks. 
Take the task of node classification as an instance, for an undirected attributed graph $\mathcal{G}=(\mathcal{A}, \mathcal{F})$ where $\mathcal{A} \in \mathbb{R}^{N \times N}$ represents the adjacency matrix of N nodes and $\mathcal{F} \in \mathbb{R}^{N \times D}$ denotes the set of node features with D dimensions.
Define a GML model $\mathcal{M}: \mathcal{G} \rightarrow \mathcal{Z}$ where $\mathcal{Z} \in[0,1]^{N \times L}$, which maps a graph $\mathcal{G}$ to probability vectors with $L$ classes.
Generally, the objective of adversarial attacks on GML models can be formulated as:
$$
    \max _{\mathcal{G}^{\prime}}\left|\underset{l \in[1, \ldots, L]}{\arg \max } \mathcal{M}\left(\mathcal{G}^{\prime}\right) \neq \underset{l \in[1, \ldots, L]}{\arg \max } \mathcal{M}(\mathcal{G})\right| \text { s.t. } d_{\mathcal{A}}\left(\mathcal{A}^{\prime}, \mathcal{A}\right) \leq \Delta_{\mathcal{A}} \text { and } d_{\mathcal{F}}\left(\mathcal{F}^{\prime}, \mathcal{F}\right) \leq \Delta_{\mathcal{F}}
$$
where $\mathcal{G}^{\prime}=\left(\mathcal{A}^{\prime}, \mathcal{F}^{\prime}\right)$ is the attacked graph, and $d_{\mathcal{A}}$ and $d_{\mathcal{F}}$ are distance metrics in the metric space $\left(\mathcal{A}, d_{\mathcal{A}}\right)$ and $\left(\mathcal{F}, d_{\mathcal{F}}\right)$.
The attacker tries to maximize the number of incorrect predictions by GML models, under the constraints $\Delta_{\mathcal{A}}$ and $\Delta_{\mathcal{F}}$. 
For instance, $\Delta_{\mathcal{A}}$ can be the limited number of modified edges and $\Delta_{\mathcal{F}}$ can be the limited range of modified features.

In this paper, we focus on graph modification attack.
Modification: Attackers modify the original graph (the same one used by defenders for training) by adding/removing edges or perturbing node features.

- Knowledge.
Attackers do NOT have access to the targeted model (including its architecture, parameters, defense mechanism, etc.). However, they can access the graph data (structure, features, labels of training data, etc.). Additionally, they have limited chances to query the model to get outputs.

***
## Graph Adversarial Attack

### Graph Modification Attack

- [Adversarial Attacks on Neural Networks for Graph Data]() KDD2018 <font color="red">Nettack</font> [解析](https://blog.csdn.net/b224618/article/details/82025371)
- [Adversarial Attacks on Graph Neural Networks via Meta Learning]() ICLR2019 <font color="red">Metattack</font>
- [Fast Gradient Attack on Network Embedding]() Arxiv2018 <font color="red">FGA</font>
- [Topology Attack and Defense for Graph Neural Networks: An Optimization Perspective]() IJCAI2019 <font color="red">TopologyAttack</font>
- [Adversarial Attack on Graph Structured Data]() ICML2018 <font color="red">RL-S2V</font>
- [Hiding individuals and communities in a social network]() natureHB2018 <font color="red">DICE</font>
- [Query-free black-box adversarial attacks on graphs]() Arxiv2020 <font color="red">STACK</font>
- [Adversarial attacks on node embedding via graph poisoning]() ICML2019 <font color="red">NEA</font>

### Graph Injection Attack
- [TDGIA: Effective Injection Attacks on Graph Neural Networks]() KDD2021 <font color="red">TDGIA</font>
- [Topology adaptive graph convolutional networks]() Arxiv2017 <font color="red">SPEIT</font>
- [Black-box Node Injection Attack for Graph Neural Networks]() Arxiv2022 <font color="red">GA2C</font>

***
## Graph inference attack
- [Model Stealing Attacks Against Inductive Graph Neural Networks]() S&P2022
- [Inference Attacks Against Graph Neural Networks]() Usenix2022
- [Stealing Links from Graph Neural Networks]() Usenix2021

***
## Graph Adversarial Defense
***
## 综述
- [Adversarial Attacks and Defenses on Graphs]() KDD2021
- [Graph Robustness Benchmark: Benchmarking the Adversarial Robustness of Graph Machine Learning]() NIPS2021