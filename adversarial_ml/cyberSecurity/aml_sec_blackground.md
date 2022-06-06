# The Background of AML in Cyber Security 

[Adversarial classification]() KDD2004
**Essentially all data mining algorithms assume that the data generating process is independent of the data miner’s activities.**
However, in many domains, including spam detection, intrusion detection, fraud detection, surveillance and counter-terrorism, this is far from the case: the data is actively manipulated by an adversary seeking to make the classifier produce false negatives.
In these domains, the performance of a classifier can degrade rapidly after it is deployed, as the adversary learns to defeat it.
We view classification as a game between the classifier and the adversary, and produce a classifier that is optimal given the adversary’s optimal strategy.
Experiments in a spam detection domain show that this approach can greatly outperform a classifier learned in the standard way, and (within the parameters of the problem) automatically adapt the classifier to the adversary’s evolving manipulations.

Effectively, spammers and data miners are engaged in a never-ending game where data miners continually come up with new ways to detect spam, and spammers continually come up with new ways to avoid detection.
Similar arms races are found in many other domains: computer intrusion detection, where new attacks circumvent the defenses put in place against old ones.
In many of these domains, researchers have noted the presence of adaptive adversaries and the need to take them into account.
The result is that the performance of deployed KDD systems in adversarial domains can degrade rapidly over time.
We believe our approach and its future extensions have the potential to significantly improve the speed and cost-effectiveness of keeping KDD systems up to date with their adversaries.
Notice that adversarial problems cannot simply be solved by learners that account for concept drift.
We first formalize the problem as a game between a cost-sensitive classifier and a cost-sensitive adversary.

[Evasion attacks against machine learning at test time]()
In security-sensitive applications, the success of machine learning depends on a thorough vetting of their resistance to adversarial data.
In one pertinent, well-motivated attack scenario, an adversary may attempt to evade a deployed system at test time by carefully manipulating attack samples.
We evaluate our approach on the relevant security task} of malware detection in PDF files, and show that such systems can be easily evaded.

Machine learning is being increasingly used in security-sensitive applications such as spam filtering, malware detection, and network intrusion detection.
Due to their intrinsic adversarial nature, these applications differ from the classical machine learning setting in which the underlying data distribution is assumed to be stationary.
To the contrary, in security-sensitive applications, samples (and, thus, their distribution) can be actively manipulated by an intelligent, adaptive adversary to confound learning.
Two approaches have previously addressed security issues in learning.
1) The min-max approach assumes the learner and attacker’s loss functions are antagonistic, which yields relatively simple optimization problems.
2) A more general game-theoretic approach applies for non-antagonistic losses; e.g., a spam filter wants to accurately identify legitimate email while a spammer seeks to boost his spam’s appeal. 
Under certain conditions, such problems can be solved using a Nash equilibrium approach.


[Omni: automated ensemble with unexpected models against adversarial evasion attack]()
To help security practitioners and researchers build a more robust model against adversarial evasion attack through the use of ensemble learning.
In studies with five adversarial evasion attacks on five security datasets.
To counter those threats, machine learning algorithms are being widely applied to security critical tasks, such as malware detection and intrusion detection.

[Cost-Aware Robust Tree Ensembles for Security Applications]() Usenix2021
Many machine learning classifiers are used in security-critical settings where adversaries actively try to evade them.
Unlike perturbing features (e.g., pixels, words) in non-security related applications, the attacker has different cost to manipulate different security features.
Moreover, many recent works focus on improving the robustness of neural network models}, whereas security applications widely use tree ensemble models such as random forest (RF) and gradient boosted decision trees (GBDT) to detect malware.
***
