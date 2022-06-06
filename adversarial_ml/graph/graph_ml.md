# Graph Machine Learning

## Material
- [GraKeL: A Graph Kernel Library in Python](https://github.com/ysig/GraKeL)

## Paper
- [Wasserstein Weisfeiler-Lehman Graph Kernels](https://proceedings.neurips.cc/paper/2019/file/73fed7fd472e502d8908794430511f4d-Paper.pdf) NIPS2019
- [Graph Kernels: A Survey]

## Algorithm
**Graph Neural Networks (GNNs)**
Representation learning of graph-structured data is challenging because both graph structure and node features carry important information. 
Graph Neural Networks (GNNs) provide an effective way to fuse information from network structure and node features.
Most of the GNNs follow a neighborhood aggregation strategy, where the model iteratively updates the representation of a node through message passing and aggregating representations of its neighbors. 
After $l$ iterations of aggregation, a node’s representation, denoted as $\mathbf{h}_{v}$, captures the structural information within its l-hop network neighborhood. 
In practice, a GNN contains several graph convolutional layers. 
Each graph convolutional layer of a GNN
$$
    \mathbf{h}_{v}^{l}=\operatorname{AGGREGATE}\left(\mathbf{h}_{v}^{l-1}, \operatorname{MSG}\left(\mathbf{h}_{v}^{l-1}, \mathbf{h}_{u}^{l-1}\right)\right), u \in \mathcal{N}(v)
$$
Note that the number of graph convolutional layers is equivalent to the l-hop network neighborhood that a GNN model can reach in the graph. 
Once trained, the GNN can map each node to an embedding vector.
These node embeddings can be directly used for downstream machine learning tasks that can be categorized into three levels, i.e., node-level (e.g., node classification), link-level (e.g., link prediction), and graph-level (e.g., graph classification).

**Inductive GNN Models**
There are two settings for training the GNNs, i.e., transductive setting and inductive setting. 
In the transductive setting, a GNN learns from both labelled and unlabelled nodes in a single fixed graph at the training time and predicts the labels of those unlabelled nodes once the training is done, e.g., vanilla graph convolutional network (GCN), DeepWalk, etc.
However, transductive GNN models must be retrained if new nodes are introduced to the graph.
A more popular one is the setting of inductive learning, where the learned GNN model can be generalized to the graphs that are previously unseen during the training procedure.
The reusable GNN model avoids time-consuming retraining if a graph includes more nodes or even subgraphs.
It facilitates the real-world practices of graph data analytics.
We therefore focus on the inductive setting in our study. We briefly introduce three widely used inductive GNN models below.

### GraphSAGE.
GraphSAGE proposed by Hamilton et al. is the first inductive GNN model.
Inspired by the Weisfeiler-Lehman test for graph isomorphism, GraphSAGE generalizes the original GCN into the inductive setting with different aggregation functions.
Take widely used mean aggregation operator as an example, GraphSAGE can be defined as follows:
$$
    \mathbf{h}_{v}^{l}=\operatorname{MEAN}\left(\mathbf{h}_{v}^{l-1} \cup\left\{\mathbf{h}_{u}^{l-1}, \forall u \in \mathcal{N}(v)\right\}\right)
$$

### Graph Attention Network (GAT).
It is straightforward to observe that GraphSAGE assigns the same weight to all neighbors (i.e., $1 / \mathcal{N}(v)$) when aggregating $v$’s neighborhood information. 
However, in practice, different nodes may play different roles in the target node embedding. 
Inspired by the attention mechanism in deep learning, Velickovic et al. propose GAT that leverages multi-head attention to learn different attention weights and pays more attention to the important neighborhoods. 
Its aggregation function can be formulated as:
\begin{equation}
    \mathbf{h}_{v}^{l}=\|_{z=1}^{Z}\left(\sum_{u \in \mathcal{N}(v)} \alpha_{u v}^{z} \cdot \mathbf{W}^{z} \cdot \mathbf{h}_{u}^{l-1}\right)
    \end{equation}
where $\|$ is the concatenation operation, $Z$ is the total number of projection heads in the attention mechanism,
$W^{z}$ is the linear transformation weight matrix, and $\alpha_{u v}^{z}$ uvis the attention coefficient calculated by the $z$-th projection head.

### Graph Isomorphism Network (GIN).
GraphSAGE can be treated as an instance of the Weisfeiler-Lehman test.
Xu et al. propose Graph Isomorphism Network (GIN) to extend GraphSAGE with arbitrary aggregation functions on multi-sets.
GIN is theoretically proven to be as powerful as the Weisfeiler-Lehman test of graph isomorphism. 
Its aggregation function can be represented as:
$$
    \mathbf{h}_{v}^{l}=\left(1+\varepsilon^{l-1}\right) \cdot \mathbf{h}_{v}^{l-1}+\sum_{u \in \mathcal{N}((v)} \mathbf{h}_{u}^{l-1}
$$
where $\varepsilon$ is a learnable parameter to adjust the weight of node $v$.
The inductive GNNs usually employ shared weight parameters and neighborhood sampling to speed up the computation.