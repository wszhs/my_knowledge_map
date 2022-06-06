# Towards A Unified Min-Max Framework for Adversarial Exploration and Robustness

## Background

### Min-Max Across Domains
&nbsp; Consider $K$ loss functions ${F_i(v)}$ (each of which is defined on a learning domain), the problem of robust learning over $K$ domains can be formulated as
$$
\underset{\mathbf{v} \in \mathcal{V}}{\operatorname{minimize}} \underset{\mathbf{w} \in \mathcal{P}}{\operatorname{maximize}} \sum_{i=1}^{K} w_{i} F_{i}(\mathbf{v})
$$
where $v$ and $w$ are optimization variables, $V$ is a constraint set, and $P$ denotes the probability simplex $\mathcal{P}=\left\{\mathbf{w} \mid \mathbf{1}^{T} \mathbf{w}=1, w_{i} \in[0,1], \forall i\right\}$'

<font color="red">**Regularized Formulation.**</font> 
 Following recent paper, we penalize the distance between the worst-case loss and the average loss over $K$ domains. This yields
 $$
    \underset{\mathbf{v} \in \mathcal{V}}{\operatorname{minimize}} \underset{\mathbf{w} \in \mathcal{P}}{\operatorname{maximize}} \quad \sum_{i=1}^{K} w_{i} F_{i}(\mathbf{v})-\frac{\gamma}{2}\|\mathbf{w}-\mathbf{1} / K\|_{2}^{2}
$$
where $\gamma > 0$ is a regularization parameter.
***