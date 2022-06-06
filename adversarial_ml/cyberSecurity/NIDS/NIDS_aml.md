# Adversarial Attacks against Network Intrusion Detection System

## Material

***
## paper 解析
[apruzzese2021modeling](paper/apruzzese2021modeling.md)

***
## Network Intrusion Detection System
### Table
|Citation|目标分类器|攻击输出|威胁模型|扰动特征|优化|名称|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|Arxiv 2019|Kitnet|Feature vector|Whitebox|Protocol statistical features|Gradient-based|<font color="red">Joseph</font> |
|Arxiv 2018|SVM, NB, MLP, LR, DT, RF, KNN |Feature vector|Graybox|Statistical and protocol header features|GAN|<font color="red">IDSGAN</font> |
|2018 S&P workshop|SVM, NB, MLP, LR, DT, RF, KNN |Feature vector|Graybox|Statistical and protocol header features|GAN|<font color="red">GANKnife</font> |
|Arxiv 2020|DNN|Feature vector|Whitebox|Protocol statistical features|Gradient-based|<font color="red">AdvEMT</font> 
|JISA2021|DNN|Feature vector|Whitebox|Protocol statistical features|JSMA|<font color="red">JSMA</font> 
|Arxiv2020|DNN|Feature vector|Graybox|Protocol statistical features|GA|<font color="red">GA</font> 
|ICTAI2019|DNN|Feature vector|Graybox|Protocol statistical features|Boundary Attack|<font color="red">Boundary Attack</font> 
|ICC2019|DNN|End-to-end|Graybox|PCAP|RL|<font color="red">RL</font> 
|RAID2021|DNN|End-to-end|Graybox|PCAP|RL|<font color="red">RL</font> 
|CCSW/CCS2020|DNN|Feature vector|Graybox|Protocol statistical features|NES BA PointwiseAttack HSJA|<font color="red">Tiki-Taka</font>
|IOT2021|kitnet|Feature vector|Graybox|Protocol statistical features|Transfer+FGSM,JSMA|<font color="red">Iot-Transfer</font>  
|Conext2019|kitnet|End-to-end|Graybox|Pcap|Gradient-based|<font color="red">PracticalAttack</font>  
|NCA2018|DNN|Feature vector|Blackbox|Protocol statistical features|Random|<font color="red">RandomAttack</font> 
|JSAC2021|Kitnet LR SVM DT RF|End-to-end|Gray+Blackbox|PCAP|GAN+PSO|<font color="red">GAN+PSO</font>  
|Arxiv2021|DNN|End-to-end|Graybox|PCAP|SeqGAN|<font color="red">SeqGAN</font> 
|ARES2019|DAGMM, AE, AnoGAN, ALAD, DSVDD, OC-SVM, IF|End-to-end|Queryefficient graybox|only nonimpactful features like send time|Gradient-based|<font color="red">AdvAnomaly</font> |
|Infocom2021|DNN|Feature vector|Whitebox|Protocol statistical features|Gradient-based|<font color="red">MANDA</font> 

### Paper

- [Rallying Adversarial Techniques against Deep Learning for Network Security]() Arxiv2019<font color="red">Joseph</font> 
- [Idsgan: Generative adversarial networks for attack generation against intrusion detection]() Arxiv2018<font color="red">IDSGAN</font>
- [Bringing a GAN to a Knife-Fight: Adapting Malware Communication to Avoid Detection]() 2018 S&P workshop <font color="red">GANKnife</font> 
- [Flow-based detection and proxy-based evasion of encrypted malware C2 traffic]()arxiv2020<font color="red">AdvEMT</font>
- [Adversarial attacks on machine learning cybersecurity defences in IndustrialControl Systems]() JISA2021 <font color="red">JSMA</font>
- [Adversarial Machine Learning in Network Intrusion Detection Systems]() Arxiv2020 <font color="red">GA</font>
- [Adversarial Attack against DoS Intrusion Detection: An Improved Boundary-Based Method]() ICTAI2019 <font color="red">Boundary Attack</font> 
- [Evading Machine Learning Botnet Detection Models via Deep Reinforcement Learning]() ICC2019 <font color="red">RL</font>
- [Crafting Adversarial Example to Bypass Flow-&ML- based Botnet Detector via RL]() RAID2021 <font color="red">RL</font>
- [Tiki-Taka: Attacking and Defending Deep Learning-based Intrusion Detection Systems]() CCSW/CCS2020 <font color="red">Tiki-Taka</font>
- [Adversarial Attacks against Network Intrusion Detection in IoT Systems]() IOT2021 <font color="red">Iot-Transfer</font>
- [Towards evaluation of nidss in adversarial setting]() Conext2019  <font color="red">PracticalAttack</font>
- [Evading botnet detectors based on flows and Random Forest with adversarial samples]()NCA2018 <font color="red">RandomAttack</font>
- [Evaluating and Improving Adversarial Robustness of Machine Learning-Based Network Intrusion Detectors]()JSAC2021 <font color="red">GAN+PSO</font>  [github](https://github.com/dongtsi/TrafficManipulator)
- [Packet-Level Adversarial Network Traffic Crafting using Sequence Generative Adversarial Networks]()Arxiv2021 <font color="red">SeqGAN</font>
- [Black box attacks on deep anomaly detectors]()ARES2019 <font color="red">AdvAnomaly</font>
- [MANDA: On Adversarial Example Detection for Network Intrusion Detection System]() Infocom2021+TDSC2022 <font color="red">MANDA</font>
- [TANTRA: Timing-Based Adversarial Network Traffic Reshaping Attack]() Arxiv2021 <font color="red">TANTRA</font>

***

## 综述
- [Modeling Realistic Adversarial Attacks against Network Intrusion Detection Systems]() Arxiv2021
- [Adversarial Machine Learning In Network Intrusion Detection Domain: A Systematic Review]() Arxiv2021
- [Adversarial Black-Box Attacks Against Network Intrusion Detection Systems: A Survey]() 
- [对抗机器学习在网络入侵检测领域的应用]() 通信学报2021
- [Improving Network Intrusion Detection Classifiers by Non-payload-Based Exploit-Independent Obfuscations: An Adversarial Approach]() Arxiv2018

## Website Fingerprinting Attacks
- [Mockingbird: Defending Against Deep-Learning-Based Website Fingerprinting Attacks with Adversarial Traces]() TIFS2020 [github](https://github.com/msrocean/mockingbird/)
- [Defeating DNN-Based Traffic Analysis Systems in Real-Time With Blind Adversarial Perturbations]() Usenix2021 [github](https://github.com/SPIN-UMass/BLANKET)