# XAI for Security Applications

## Material
- [求解网络安全问题的可解释机器学习](https://toooold.com/2022/02/13/explainable_w_shap_cn.html)
## Paper
- [DeepAID: Interpreting and Improving Deep Learning-based Anomaly Detection in Security Applications]() CCS2021
- [Interpreting Deep Learning-Based Networking Systems]() Sigcomm2020
- [lemna:Explaining deep learning based security applications]() CCS2018
- [Can We Trust Your Explanations? Sanity Checks for Interpreters in Android Malware Analysis]() TIFS2021
- [CADE: Detecting and Explaining Concept Drift Samples for Security Applications]() Usenix2021
- [Adversarial XAI Methods in Cybersecurity]() TIFS2021
- [Evaluating Explanation Methods for Deep Learning in Security]() EuroS&P2020
- [Contextual outlier interpretation]() IJCAI2018


## Background
[Lemna: Explaining deep learning based security applications]() ccs2018
In recent years, Deep Neural Networks have shown a great potential to build security applications.

So far, researchers have successfully applied deep neural networks to train classifiers for malware classification, binary reverse-engineering and network intrusion detection, which all achieved an exceptionally high accuracy.
While intrigued by the high-accuracy, security practitioners are concerned about the lack of transparency of the deep learning models and thus hesitated to widely adopt deep learning classifiers in security and safety-critical areas.
Most existing works focus on non-security applications such as image analysis or natural language processing (NLP).
Unfortunately, existing explanation methods are not directly applicable to security applications.

However, CNN model is not very popular in security domains.
Security applications such as binary reverse-engineering and malware analysis either have a high-level feature dependency (e.g, binary code sequences), or require high scalability}.
This might be acceptable for image analysis, but can cause serious troubles in security applications.
In this paper, we seek to develop a novel, high-fidelity explanation method dedicated for security applications.

[Evaluating explanation methods for deep learning in security]() EuroS\&P2020
Over the last years, deep learning has been increasingly recognized as an effective tool for computer security.
In contrast to other application domains of deep learning, computer security poses particular challenges for the use of explanation methods.
explanation methods in security do not only need to be accurate but also **satisfy security-specific requirements, such as complete and robust explanations.**
Our work provides a bridge between deep learning in security and explanation methods developed for other application domains of machine learning.
We define the completeness, stability, robustness, and efficiency of explanations as security criteria.


[DeepAID: Interpreting and Improving Deep Learning-based Anomaly Detection in Security Applications]() CCS2021
Unsupervised Deep Learning (DL) techniques have been widely used in various security-related anomaly detection applications, owing to the great promise of being able to detect unforeseen threats.
Unfortunately, existing interpretation approaches are proposed for supervised learning models and/or non-security domains, which are unadaptable for unsupervised DL models and fail to satisfy special requirements in security domains.
DeepAID can provide high-quality interpretations for unsupervised DL models while meeting several special requirements in security domains.

deep learning (DL) has completely surpassed traditional methods in various domains, owing to the strong ability to extract high-quality patterns and fit complex functions.
Consequently, unsupervised deep learning models are more desirable in security domains} for owing both the ability to detect unforeseen anomalies and high detection accuracy.
So far, unsupervised deep learning models have been applied in various security-related anomaly detection applications, such as network intrusion detection, system log anomaly detection, advanced persistent threat (APT) detection, domain generation algorithm (DGA) detection and web attack detection.
While demonstrated great promise and superior performance, DL models, specifically Deep Neural Networks (DNN) are lack of transparency and interpretability of their decisions.


[CADE: Detecting and Explaining Concept Drift Samples for Security Applications]() Usenix2021
Concept drift poses a critical challenge to deploy machine learning models} to solve practical security problems.
Deploying machine learning based security applications can be very challenging due to concept drift. 
Whether it is malware classification, intrusion detection, or online abuse detection, learning-based models work under a “closed-world” assumption}, expecting the testing data distribution to roughly match that of the training data.

