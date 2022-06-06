# Adversarial Machine Learning for Cyber Security 


In contrast to image-based attacks, the cybersecurity domain can modify feature vectors $\mathbf{x}$ (e.g., flow statistical features) or directly modify source files $\mathbf{z}$ (e.g. PCAP and PE file). 
However, the modification process must maintain the file's executability and maliciousness.

In general, there are two approaches, the first of which is to modify the feature vectors $\mathbf{x}$ directly and then map the feature vectors $\mathbf{x}$ back to the problem-space $\mathcal{Z}$ through a remapping function $\mathcal{M}$. 
The second option is to modify the source file $\mathbf{z}$ directly.

## Restricted Feature-Space Attacks
For a given detector $f$ and a given input object $z$ with feature representation $x \in \mathcal{X} (f(x)=y)$, 
the attacker attempts to minimize the distance $l$ between and $\mathbf{x}$ and $\hat{\mathbf{x}}$ in the feature-space, such that the resulting adversarial examples $\hat{\mathbf{x}} \in \mathcal{X}$ in the feature-space can misclassify the classification models $f$ .
And she uses remapping function $\mathcal{M}(\hat{\mathbf{x}})$ to project $\hat{\mathbf{x}}$ back to legal space for crafting adversarial features $\hat{\mathbf{x}}$, which ensures that $\hat{\mathbf{x}}$ can be transformed into a legal $\hat{\mathbf{z}}$.

We next introduce the remapping function $\mathcal{M}$ for intrusion detection and malware detection in detail.

### 1 Remapping Function

#### Network Intrusion Detections (NIDSs)
When evaluating flow-based NIDS in an adversarial setting, we group the features fed into them into four different categories.  
First of all, some of the flow-based features cannot be changed because the attacker doesn't have control over them because they are extracted from the victim's traffic. 
Additionally, some features are dependent on others. 
It is also possible to determine the mean of forward packet payloads using two other features, the total length of forward packet payloads and the number of forward packets.
Another type of feature depends on the actual packets of the flow and cannot be calculated based on the value of other features (e.g., the standard of packet payloads in forward direction). 
As a result, we group flow features into four categories.
  - Features that shouldn't be changed because they are extracted from packets flowing backwards (victim packets).
  - Features that can be transformed individually by using the legitimate transformations. They include total forward packets, total push flags in the forward direction, maximum packet interarrival time (IAT) in the forward direction, etc.
  - Features that depend on or can be calculated directly by a subset of the second group.
  - The sequence of packets affects the values of features that cannot be directly recalculated based on independent features.

We group flow-based features into the following four groups.
The first category of features is those we can modify independently.
The second group of features is dependent on other features. For example, the mean of packet payloads in the forward direction can be calculated using two other features: the total length of forwarding packets payloads and the total number of forwarding packets.
Third, some features cannot be changed because attackers cannot control them.
There is another type of feature in which their value depends on the actual packets of the flow and cannot be calculated by the value of other features.
We directly modify the features that can be modified independently and calculate the second group of features instantly, without modifying the last two groups of features.

[Generating Practical Adversarial Network Traffic Flows Using NIDSGAN]()
We consider traffic to be adversarial if it is undetectable by the NIDS and we consider traffic to be constraint-compliant if it is realizable in practice.
The former is readily enforced by GANs and we enforce the latter through two types of constraints: (1) inherent constraints, and (2) valid ranges.

The inherent constraints of network traffic flow are characterized by the traffic’s protocol and attack type. 
Without constraints, the adversary could perturb $2^n$ (n-number of feature of traffic flow) different subsets of all the features.
However, the network domain allows a very small number of subsets as valid perturbation for a given network traffic flow because the network traffic flows must be realizable in the real world.
For instance, a network TCP flow (packet) may only certain flags and services, and UDP flows may have others.

The constraint of valid ranges is critical for the adversary. 
Each feature in the adversarial traffic flow has its corresponding valid range. 
Out-of-range feature values render the attack invalid, as the traffic (1) loses its attack semantics, and (2) cannot be realized in practice.
Therefore, extreme values out of the feature distributions can easily expose the attack traffic to NIDS.
We explain the two constraints in detail along with their implementations in the following.

**1 Inherent constraints**
 - Attack type dictates that a set of feature values of traffic that must be kept constant. This preserves the underlying semantics of an attack.
- Protocol type dictates that a certain subset of features must be zero, as some features are protocol-specific (such as TCP flags). Also, a certain protocol is allowed to have only certain flags and services for feature values.
- One-hot features converted from categorical must not be perturbed. This ensures perturbations applied by GANs result in realizable traffic.


**2 Valid ranges**
For each type of attack, we find the minimum and the maximum value of each feature and define the range between the minimum and maximum as the valid range.
Then, after applying the perturbation to original attack traffic during the process of crafting adversarial examples, we enforce the valid range for each feature of attack traffic by clipping. 
Figure 6 shows that valid ranges of each class in NSL-KDD dataset for a particular feature $x_1$. 
For example, if the feature value of $x_1$ of a DoS attack traffic becomes $x_{1}+\Delta x_{1}<d_{\min }$ minafter a perturbation is applied, then we clip the value and replace it with $d_{min}$.

**Realizing Adversarial Flows**
The last phase of our attack is to execute PCAPs over the network such that they bypass NIDS with real traffic, i.e., realize the malicious intent of the adversarial example. 
There are several technical challenges in creating “playback” for an adversarial flow , including packet timing, traffic shaping, and other techniques. 
As it turns out there are several well-known tools that serve this purpose, such as tcpreplay, Bit-Twist, and Packet Player. 
They could be used to replay edited PCAP data to generate the adversarial network traffic with expected high fidelity. 
Moreover, some works have demonstrated success in exclusively perturbing statistical information (i.e., perturbations to features that have no constraints) so that network packets can be replayed to bypass NIDS.
Either of these frameworks, with some necessary extensions beyond perturbations on statistical features, could be leveraged by our approach to produce a network flow that is representative of the generated adversarial examples. 
We are exploring executing these tools on the real networks (which is a complex and lengthy exercise), but for brevity we defer those experiments to our future work.
We defer interested readers to Table 20 in the Appendix on how to apply each of the cited techniques to realize our adversarial flows.

#### Malware Detections
Consider the two simplest examples of malware detection: append attacks and slack attacks.
Malware's remapping function is better defined than network traffic.
An append attack adds some specially trained bytes at the end of the file to trick the classifier.
Slack attacks look for loose pieces of samples (binaries/script files/attack traffic), and insert perturbations in those locations. 
Thus, the remapping function allows only the tail and loose fragments to modify the value, and the rest remains the same.
For other scenarios, the remapping function is similar to these two.


### 2 Learning constraints
- [On the Robustness of Domain Constraints]() CCS2021
<!-- 直接从任意域学习领域约束是一个挑战的过程，域包含多层复杂的抽象。幸运的是，在形式化逻辑领域已经有数据驱动的方法去学习域约束。这种方法来自于PAC学习理论。我们使用Valiant描述了从数据中学习布尔约束的设置。Valiant限制理论以一个简单优雅的表示形式化了约束，能够融入对抗生成算法中。然后，我们使用DPLL算法投影对抗样本进入域约束空间。DPLL可以迭代地构建给定表达式的候选解决方案。同时针对连续特征值，使用OPTICS算法处理。 -->
In many domains, there are regions for which samples do not exist (e.g., UDP flows in the network domain do not have TCP flags). The structures and rules that define these regions may be complex and thus, we need a general approach to learn how domains are partitioned into valid and invalid regions.
We leverage an algorithm from PAC learning, Valiant’s algorithm,

***
## End-to-end Problem-space
In contrast with the aforementioned feature-space attacks, 
the problem-space attacks refer to the adversarial attack that is performed in the problem-space $\mathcal{Z}$, i.e., how to “modify” the real-world input $\mathbf{z} \in \mathcal{Z}$ with a minimal cost (e.g., executables, source code, pcap, etc.) such that the generated AEs $\hat{\mathbf{z}} \in \mathcal{Z}$ in the problem-space can misclassify the target model $f$.

### NIDS
[Evading machine learning botnet detection models via deep reinforcement learning]() ICC2019
The agent takes an available modification to alter the flow sample through the policy in each iteration.
There are a number of actions that can be made to a flow, which will not change the original function and not break the flow file format.
These actions are able to change the communication pattern in temporal and spatial dimension.
Some of the actions selected in this paper include:
- Modifying the timestamp of the first packet. The timestamp (the first 4 bytes in a packet header) increases or decreases for 0.1 to 0.3ms randomly.
- Adding the length of the packet payload. The modified packet is randomly added 1 to 5 0x00 bytes after the payload. The total amount of modified packets in a flow will be less than 30\%.
- Appending a new 4 bytes packet composed of 0x00.
- Appending a new 4 bytes packet composed of a sequence of random characters.
- Appending a benign packet that extracted from a normal flow.

[Crafting Adversarial Example to Bypass Flow-ML- based Botnet Detector via RL]() RAID2021

When constructing new packets, we mainly consider three attributes: **timestamp, direction, and packet size**.

- Change the duration
  - Withhold the final TCP FIN packet for randomly 1-3s
- Change the time interval
  - Add a forward packet with an interval of 20s at the end
  - Add a backward packet with an interval of 20s at the end
- Change the image characteristics: (for the DNN model) pay attention to the location and content of the packet
  - Add an empty packet at random location (0 < loc < 8)
  - Add a random packet at random location (0 < loc < 8)
  - Add a full 0 packet at random location (0 < loc < 8)

- Change the statistical characteristics
  - Add a forward large-size 0 packet
  - Add a backward large-size 0 packet
  - Add a forward avg-size packet with no content at the head
  - Add a backward avg-size packet with no content at the head
  - Add a forward empty packet
  - Add a backward empty packet

- Change the packet length
  - Add a random length of 0 at the end of the packet with a probability of 0.2
  - Select two packets without payloads, add the character ’0’to avg-size

[Evaluating and Improving Adversarial Robustness of Machine Learning-Based Network Intrusion Detectors]() JSAC2021
To solve the evasive traffic mutation, we first design some basic parameterized mutation operators. 
The mutation operators should be able to affect as many types of features as possible to be generic, and also should be functionality-preserving.
- Temporal features are related to the timing of traffic, such as the inter-arrival time of packets.
- Spatial features are further divided into Global and Local Spatial features:
  - Global Spatial features are the overall characteristics of traffic (for example, the volume of traffic, such as the number of bytes or the total number of packets).
  - Local Spatial features are related to the content of packets. Since we only focus on the packet headers and certain protocols, the types of Local Spatial features are limited.


Then, we design mutation operators which can affect all summarized high-level features (Fig. 7 in Appendix B provides an intuitive illustration) while preserving the functionality.
Specifically, they consist of modifying original malicious traffic and injecting/adjusting crafted stealthy traffic:

- Original Malicious Traffic Modification}
    We ensure that the original traffic will not be deleted and the order of packets will not be changed. Hence the only mutation operator is:
  - Altering the interarrival time of packets in original traffic

- Crafted Stealthy Traffic Injection:
    It is non-trivial to determine packer headers’ content of crafted traffic. 
  - Firstly, we can only craft traffic sent from the attacker, and some fields (MAC/IP/port) in crafted packets need to be consistent with that of original packets nearby; otherwise the crafted packets cannot affect features extracted from original packets.
  - Secondly, the assignment of other fields in header must meet the following requirements: 1) it will not compromise the maliciousness of the original traffic; 2) it will not cause the protocol semantics or communication violation (such as connection breakdown of TCP traffic); 3) it will not induce responses by the victim (for stealth and consistency of replay).

In light of these requirements, we list optional methods for generating crafted traffic in Table III. We note that a previous method used in [25] by modifying TTL requires the knowledge of the victim’s network topology, which is extremely strict.

Hence, we propose other methods for different types of traffic without additional knowledge.
- Crafted Traffic Adjustment
  - Altering the interarrival time of packets in crafted traffic
  - Altering the protocol layer of packets in crafted traffic
  - ltering the payload size of packets in crafted traffic






## Verification of Malicious Functionality
However, doing so would defeat the purpose of adversarial attacks, which involves the application of small and imperceptible modifications.
Hence, the modified samples need to only slightly differ from their original variants, and they must also maintain their underlying malicious logic without triggering other detection mechanisms.
While such conditions are verifiable for attacks performed in the problem-space, attacks performed at the feature-space require additional verification steps.
Furthermore, when simulating attacks at the feature-space it is also needed to check that all inter-dependencies between features are maintained, and that the feature values of the perturbed samples do not result in impossible numbers: for example, the size of a TCP packet cannot exceed 64KBytes, while some network flow collectors have fixed thresholds for the maximum flow duration.

[Evaluating and Improving Adversarial Robustness of Machine Learning-Based Network Intrusion Detectors]()
To be rigorous, although we guarantee the mutation operators in Section VI-A will not compromise the malicious functionality of original traffic, we still verify the malicious functionality of the mutated traffic in all six traffic sets.
To measure the malicious functionality, we use three types of indicators: attack effect, malicious behavior and attack efficiency, and compare them in original and mutated traffic.
Take Botnet as an example to illustrate the three indicators: In our selected traffic, an attacker used a malware called Mirai to scan IoT devices in the LAN and successfully scanned 8 open devices. 
In this scenario, attack effect is the final result of the attack, which is that 8 devices were successfully scanned. 
Malicious behavior contains all offensive behaviors regardless of whether they eventually affect, which is the number of scans. 
Attack efficiency is related to the elapsed time of the attack. 
Obviously, attack effect has a greater impact on functionality than malicious behavior, and the change rate of attack efficiency must be within the attacker’s overhead budget $l_t$.

We use VMs and Dockers to simulate the experimental testbed for each traffic set by referring to their papers.
Then we use Tcpreplay and Tcplivereplay to replay original and mutated attack traffic in the testbed and observe the three indicators. 
Since different attacks have diverse functionalities, the specific meanings of three indicators are case-by-case, making the validation experiment straightforward but tedious, so we put the details in Appendix E and leave the result here: Mutated traffic generated through our evasion attack can preserve the malicious functionality.
Specifically, attack effect keeps unchanged in all cases and malicious behavior is reduced only in DoS/DDoS attacks (attack bandwidth is decreased due to increased time interval, but this reduction does not exceed the attacker’s budget ($l_t$).
As for attack efficiency, although our method may slow down some kinds of attack, the change rate of elapsed time is always $<l_t$.

Detailed results of verifying malicious functionality of all six attack traffic sets are listed in Table IX.
Note that, although the attack effect cannot be measured in some cases, in fact the results of attack effect are generally the same as malicious behavior.
For example, in Brute Force, we do not know the true password of the victim server, but if we guarantee that all the original password attempts exist in the mutated traffic, then obviously the final result is the same.
