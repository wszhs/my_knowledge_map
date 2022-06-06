# Game-theoretic Explaination

## Material
- [神经网络的博弈交互解释性专栏](https://zhuanlan.zhihu.com/p/264871522/)
- [博弈论&夏普利值！提高机器学习可解释性的新方法！](https://zhuanlan.zhihu.com/p/99048980)
- [SHAP知识点全汇总](https://zhuanlan.zhihu.com/p/85791430)

## Survey
- [Explaining by Removing: A Unified Framework for Model Explanation]() JMLR2021

## Paper
- [Explaining instance classifications with interactions of subsets of feature values]()<font color="red">Interactions Methods for Explanations (IME)</font>
- [Algorithmic transparency via quantitative input influence: Theory and experiments with learning systems]() S&P2016 <font color="red">Quantitative Input Influence (QII)</font>
- [A Unified Approach to Interpreting Model Predictions]() NIPS2017 <font color="red">SHAP</font>  
- [Improving KernelSHAP: Practical Shapley Value Estimation using Linear Regression]() PMLR2021 <font color="red">KernelSHAP</font> 
- [From Local Explanations to Global Understanding with Explainable AI for Trees]() nature-ml2020 <font color="red">TreeSHAP,LossSHAP</font> 
- [Understanding global feature contributions with additive importance measures]() NIPS2020 <font color="red">SAGE</font> 
- [Analysis of regression in game theory approach]() <font color="red">Shapley Effects</font>
- [FastSHAP: Real-time shapley value estimation]() ICLR2022 <font color="red">FastSHAP</font>
- [Distribution-Free Predictive Inference For Regression]() <font color="red">LOCO</font>
- [Model Class Reliance: Variable Importance Measures for any Machine Learning Model Class, from the "Rashomon" Perspective]() <font color="red">Permutation Test</font>
- [An information-theoretic perspective on model interpretation]() <font color="red">Learning to Explain (L2X)</font>
- [How interpretability methods can learn to encode predictions in their interpretations]() <font color="red">REAL-X</font>
- [INVASE: Instance-wise variable selection using neural networks]() <font color="red">Instance-wise Variable Selection (INVASE)</font>
- [Why should I trust you? Explaining the predictions of any classifier]() <font color="red">LIME</font>
- [Learning important features through propagating activation differences]() <font color="red">DeepLIFT</font>
- [Interpretable explanations of black boxes by meaningful perturbation]() <font color="red">Meaningful Perturbations (MP)</font>

## Algorithm

|Method|Removal|Behavior|Summary|
|:-:|:-:|:-:|:-:|
|SHAP|Marginalize(conditional/marginal)|Prediction|Shapley value|
|KernelSHAP|Marginalize(marginal)|Prediction|Shapley value|
|TreeSHAP|Tree distribution|Prediction|Shapley value|
|LossSHAP|Marginalize(conditional)|Prediction loss|Shapley value|
|SAGE|Marginalize(conditional)|Dataset loss(label) loss|Shapley value|
|Shapley Effects|Marginalize(conditional)|Dataset loss(output) loss|Shapley value|
|Permutation Test|Marginalize(conditional)|Dataset loss(label) loss|Remove individual|
|LiME(Tabular)|Marginalize(replacement dist.)|Prediction|Linear model|

