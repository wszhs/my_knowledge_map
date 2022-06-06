# Min-Max Optimization
## Formula
$$
\min _{\mathbf{x} \in \mathbb{R}^{n}} \max _{\mathbf{y} \in \mathbb{R}^{m}} f(\mathbf{x}, \mathbf{y})
$$
***
## Min-Max Optimization with Gradients
### Paper
- [On Solving Minimax Optimization Locally: A Follow-the-Ridge Approach](https://arxiv.org/pdf/1910.07512.pdf) ICLR2020 <font color="red">Follow the ridge</font> 

- [Learning a Minimax Optimizer: A Pilot Study](https://openreview.net/pdf?id=nkIDwI6oO4_) ICLR2021
- [Non-convex min-max optimization: Provable algorithms and applications in machine learning]()

### Algorithm

1. <font color="red">GDA</font> 
    ``` python
    # GDA
    def gda(z_0, alpha=0.05, num_iter=100,target=None):
        z = [z_0]
        grad_fn = grad(target)
        print(z[-1])
        for i in range(num_iter):
            g = grad_fn(z[-1])
            z1 = z[-1] + g*np.array([-1,1])*alpha
            z.append(z1)
        z = np.array(z)
        return z
    ```

2. <font color="red">Extra Gradient</font> 
3. <font color="red">Optimistic Gradient</font> 
4. <font color="red">Consensus Optimization</font> 
5. <font color="red">Symplectic gradient adjustment</font> 
6. <font color="red">Follow the ridge</font> 
   <!-- ``` python
   # Follow the ridge
    def follow(z_0, alpha=0.05, num_iter = 100,target=None):
        z = [z_0]
        grad_fn = grad(target)
        hessian = jacobian(grad_fn)
        for i in range(num_iter):
            g = grad_fn(z[-1])
            H = hessian(z[-1])
            v = np.array([g[0], -g[1]-H[0,1]*np.squeeze(pinv(H[1:,1:]))*g[0]])
            z1 = z[-1] - alpha*v
            z.append(z1)
        z = np.array(z)
        return z
   ``` -->
    <!-- $$
    \mathbf{x}_{t+1} \leftarrow \mathbf{x}_{t}-\eta_{\mathbf{x}} D_{\mathbf{x}} f\left(\mathbf{x}_{t}, \mathbf{y}_{t}\right)
    $$
    $$
    \mathbf{y}_{t+1} \leftarrow \mathbf{y}_{t}-\eta_{\mathbf{y}} \nabla_{\mathbf{y}} g\left(\mathbf{x}_{t}, \mathbf{y}_{t}\right)+\eta_{\mathbf{x}}\left(\nabla_{\mathbf{y y}}^{2} g\right)^{-1} \nabla_{\mathbf{y} \mathbf{x}}^{2} g D_{\mathbf{x}} f\left(\mathbf{x}_{t}, \mathbf{y}_{t}\right)
    $$ -->


***

## Min-Max Optimization without Gradients
### Paper
- [Min-Max Optimization without Gradients: Convergence and Applications to Black-Box Evasion and Poisoning Attacks]() ICML2020

- [Towards A Unified Min-Max Framework for Adversarial Exploration and Robustness](https://openreview.net/pdf?id=S1eik6EtPB) NIPS2021 [analyze](paper/wang2019towards.md)


- [Robust optimization over multiple domains]() AAAI2019
### Algorithm
1. <font color="red">ZO-Min-Max</font> 

***
## Bayesian Optimization

### Paper
- [Approximating Nash Equilibria for Black-Box
Games: A Bayesian Optimization Approach]() Arxiv2018
- [A Bayesian optimization approach to find Nash equilibria]() Journal of Global Optimization2019

- [Adversarially robust optimization with gaussian processes]() NIPS2018 <font color="red">STABLEOPT</font> 

### Algorithm

1. <font color="red">STABLEOPT</font> 