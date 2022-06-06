# Threat Model
## Attackers's Goals}
In a security system (e.g., intrusion detection or malware detection), the attackers' goal is to evade the detectors during the testing phase.
Specifically, the attackers might want a specific class ("malicious") classified as another class ("benign").

***

## Attackers's Knowledge
Based on the prior knowledge of the attacker, there are four types of attacks on ML-based security detectors: transparent-box attacks, white-box attacks, gray-box attacks, and black-box attacks.

- Transparent-Box attacks: In this case, the adversary has complete knowledge about the system architecture $f$ and parameters $\theta$, including both white-box knowledge and knowledge about the defense methods used by a defender. Such knowledge can assist the attacker in choosing an adaptive attack that would be capable of bypassing the specific defense mechanism.
- White-Box attacks: The attackers know every detail about the machine learning model, including its architecture $f$ and parameters $\theta$, and by calculating the optimization problem based on the ML model, the adversarial examples $\hat{\mathbf{z}}$ can be generated easily.
- Gray-Box attacks: Gray-box attacks assume that the attack does not know the specific details of the model, but knows part of the information, such as the feature extractor $\phi$, which can generate an adversarial example by querying the output of the target model $f$.
- Black-Box attacks: In a more practical setting, we assume that the attacker does not have any or very limited knowledge about the target model $f$ nor its feature extractor $\phi$.
In this case, the attacker can only build a substitute model $f'$ and a substitute feature extractor $\phi'$  based on domain knowledge.

***
## Practical Threat Model
[Modeling Realistic Adversarial Attacks against Network Intrusion Detection Systems]()
To model the **realistic capabilities** of an attacker, we introduce the concept of “power” that denotes how much control the attacker has on the target detection system.
We identify five elements on which the attacker has power:
- **Training Data**. It represents the ability to access the dataset used to train the ML-NIDS. It can come in the form of read, write, or no access at all.
- **Feature Set**. It refers to the knowledge of the features analyzed by the ML-NIDS to perform its detection. It can come in the form of none, partial or full knowledge.
- **Detection Model**. It describes the knowledge of the (trained) ML model integrated into the NIDS that is used to perform the detection. This knowledge may be none, partial or full.
- **Oracle**. This element denotes the possibility of obtaining feedback from the output produced by the ML-NIDS to an attacker’s input. This feedback can be limited, unlimited or absent.
- **Manipulation Depth**. It describes the nature of the adversarial manipulation, that may modify the analyzed traffic (problem space) traffic level or one or more features (feature space).


### 1 Training data.
We can conclude that acquiring any form of reliable access (either read or write) to the training data used to train (or re-train) the ML-model is not very realistic.
Power on the training data comes in two different forms: read access, which may allow an attacker to reproduce the training phase and obtain a similar detector to the one adopted by the organization; or write access, which can be leveraged to perform poisoning attacks by either injecting new data, or modifying existing samples.

### 2 Detection Model.
In summary, it is unlikely that an adversary may get power on the internal configuration of the ML model (which is the case of white-box attacks).

### 3 Feature Set.
However, attackers may leverage their \hl{domain expertise} to guess which features are likely to be analyzed by the detector.
Therefore, it is reasonable to assume that expert attackers will leverage such intelligence to estimate with high probability some features utilized by the actual detector, which will impact the performance.

In summary, it is realistic to assume an attacker that \hl{has some power on the feature-set} used by the MLNIDS; having complete knowledge on this element is a tough challenge. Finally, it is unlikely that an attacker is completely oblivious of the composition of the feature-set.

### 4 Oracle.
In the specific case of ML-NIDS, to obtain oracle power the attacker faces two obstacles: \hl{not triggering other detection mechanisms}, and \hl{extracting meaningful information from the input/output feedback}.
- **Avoiding Detection**: Modern organizations protect their networks through multiple defensive layers. Hence, an attacker must operate in such a way to avoid being detected by these additional detection schemes.
To this purpose, the attacker should aim at \hl{minimizing the amount of queries} issued to the target NIDS, since each query requires to create and send some additional anomalous traffic to the target network.
Furthermore, these queries should be performed in a \hl{low-and-slow} approach because excessive queries in a short time frame may easily trigger alerts by detection systems that leverage simple statistical approaches to model the normal network traffic.
In summary, an attacker can interact with the NIDS by sending some queries, but any rational attacker will try its best to \hl{limit the number of interactions}.
- **Acquiring Feedback** : The output of the detection may not be directly observable by an attacker.
When a NIDS identifies a malicious event, it generates alerts which are only notified to security administrator through the NIDS console.
Thus, in these circumstances the adversary must rely on the defender’s reactions, which is an unreliable attacking approach that may require long timespans. We identify three situations where the attacker can directly observe some information about the classification output generated by the NIDS after a given query.
  - The first scenario requires the NIDS to be integrated in some \hl{reactive defensive} mechanism that is able to automatically stop the detected malicious traffic. Acquiring such information requires significant intelligence expertise.
  - The second scenario requires the attacker to gain access to the NIDS logs or its console, but this is unlikely because such data are accessible only by the NIDS administrator.
  - The third scenario requires the organization to rely on a commercial NIDS. In a similar case, the attackers could acquire the same NIDS and deploy it in a controlled environment where they have complete freedom: they may still be unable to determine the inner configurations of the ML components, but they would not be subject to any limitations of the query.
All these requirements make this scenario feasible only for skilled and highly motivated adversaries.


### 5 Manipulation Depth.
The last element on which the attacker has power involves the data manipulation capabilities: the (adversarial) perturbations can be introduced either at the raw traffic level, or after the network data is being transformed into its higher-level feature representation.
In summary, feature-space attacks are a higher-level abstraction of the attacker’s workflow focusing only on its intended results, whereas problem-space attacks involve the reproduction of the entire malicious procedure.
For these reasons, attacks at the problem-space are more representative of a realistic scenario. The only way in which an adversary could perform a real feature-space attack is by manipulating the conversion of the raw traffic data into its feature representation, which requires full power on the ML component.
