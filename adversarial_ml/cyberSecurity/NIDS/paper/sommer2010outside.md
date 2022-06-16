<!-- vscode-markdown-toc -->
* 1. [摘要](#摘要)
* 2. [引言](#-1)
* 3. [入侵检测中的机器学习](#-1)
* 4. [使用机器学习的挑战](#-1)
	* 4.1. [孤立点检测](#-1)
	* 4.2. [错误的高成本](#-1)
		* 4.2.1. [产品推荐](#-1)
		* 4.2.2. [OCR](#OCR)
		* 4.2.3. [垃圾邮件检测](#-1)
	* 4.3. [语义鸿沟](#-1)
	* 4.4. [网络流量的多样性](#-1)
	* 4.5. [评估的困难](#-1)
		* 4.5.1. [Difficulties of Data](#DifficultiesofData)
		* 4.5.2. [Mind the Gap:](#MindtheGap:)
		* 4.5.3. [Adversarial Setting:](#AdversarialSetting:)
* 5. [使用机器学习的建议](#-1)
	* 5.1. [Understanding the Threat Model](#UnderstandingtheThreatModel)
	* 5.2. [Keeping The Scope Narrow](#KeepingTheScopeNarrow)
	* 5.3. [Reducing the Costs](#ReducingtheCosts)
	* 5.4. [Evaluation](#Evaluation)
* 6. [结论](#-1)

<!-- vscode-markdown-toc-config
	numbering=true
	autoSave=true
	/vscode-markdown-toc-config -->
<!-- /vscode-markdown-toc -->
# Outside the closed world: On using machine learning for network intrusion detection

##  1. <a name='摘要'></a>摘要
In network intrusion detection research, one popular strategy for finding attacks is monitoring a network’s activity for anomalies: deviations from profiles of normality previously learned from benign traffic, typically identified using tools borrowed from the machine learning community.
However, despite extensive academic research one finds a striking gap in terms of actual deployments of such systems: compared with other intrusion detection approaches, machine learning is rarely employed in operational “real world” settings.
We examine the differences between the network intrusion detection problem and other areas where machine learning regularly finds much more success. 
Our main claim is that the task of finding attacks is fundamentally different from these other applications, making it significantly harder for the intrusion detection community to employ machine learning effectively. 
**We support this claim by identifying challenges particular to network intrusion detection, and provide a set of guidelines meant to strengthen future research on anomaly detection**.
在网络入侵检测研究中，一种流行的发现攻击的策略是监控网络的异常活动：与之前从良性流量中学习到的正常配置文件的偏差，通常使用从机器学习社区借来的工具来识别。
然而，尽管进行了广泛的学术研究，但人们发现在此类系统的实际部署方面存在显着差距：与其他入侵检测方法相比，机器学习很少用于可操作的“现实世界”环境中。
我们研究了网络入侵检测问题与机器学习经常取得更大成功的其他领域之间的差异。
我们的主要主张是，发现攻击的任务与这些其他应用程序根本不同，这使得入侵检测社区更难有效地使用机器学习。
**我们通过识别网络入侵检测特有的挑战来支持这一主张，并提供一套旨在加强未来异常检测研究的指导方针**。

***

##  2. <a name='-1'></a>引言
Traditionally, network intrusion detection systems (NIDS) are broadly classified based on the style of detection they are using: systems relying on misuse-detection monitor activity with precise descriptions of known malicious behavior, while anomaly-detection systems have a notion of normal activity and flag deviations from that profile.
Both approaches have been extensively studied by the research community for many years. However, in terms of actual deployments, we observe a striking imbalance: in operational settings, of these two main classes we find almost exclusively only misuse detectors in use—most commonly in the form of signature systems that scan network traffic for characteristic byte sequences.
传统上，网络入侵检测系统 (NIDS) 根据其使用的检测方式进行广泛分类：系统依赖于误用检测监控活动，并精确描述已知的恶意行为，而异常检测系统具有正常活动的概念和 标记与该配置文件的偏差。
多年来，研究界对这两种方法都进行了广泛研究。 
然而，在实际部署方面，我们观察到一个显着的不平衡：在操作设置中，在这两个主要类别中，我们发现几乎完全只有误用检测器在使用——最常见的形式是扫描网络流量以查找特征字节序列的签名系统。

This situation is somewhat striking when considering the success that machine-learning—which frequently forms the basis for anomaly-detection—sees in many other areas of computer science, where it often results in large-scale deployments in the commercial world. Examples from other domains include product recommendations systems such as used by Amazon [3] and Netflix [4]; optical character recognition systems (e.g., [5], [6]); natural language translation [7]; and also spam detection, as an example closer to home。
考虑到机器学习（经常构成异常检测的基础）在计算机科学的许多其他领域中取得的成功，这种情况有些惊人，它通常会在商业世界中进行大规模部署。 
其他领域的例子包括亚马逊 [3] 和 Netflix [4] 使用的产品推荐系统； 光学字符识别系统（例如，[5]、[6]）； 自然语言翻译[7]； 以及垃圾邮件检测。

In this paper we set out to examine the differences between the intrusion detection domain and other areas where machine learning is used with more success.
Our main claim is that the task of finding attacks is fundamentally different from other applications, making it significantly harder for the intrusion detection community to employ machine learning effectively. 
We believe that a significant part of the problem already originates in the premise, found in virtually any relevant textbook, that **anomaly detection is suitable for finding novel attacks**; 
we argue that this premise does not hold with the generality commonly implied. 
Rather, the strength of machine-learning tools is finding activity that is similar to something previously seen, without the need however to precisely describe that activity up front (as misuse detection must).
**在本文中，我们着手研究入侵检测领域与其他更成功地使用机器学习的领域之间的差异**。 
我们的主要主张是发现攻击的任务与其他应用程序根本不同，这使得入侵检测社区更难有效地使用机器学习。 
我们认为，问题的很大一部分已经起源于几乎所有相关教科书中都发现的前提，即异常检测适用于发现新的攻击； 
我们认为，这个前提与通常隐含的普遍性不成立。 
相反，机器学习工具的优势在于发现与之前看到的活动相似的活动，而无需预先精确描述该活动（因为误用检测必须如此）。

In addition, we identify further characteristics that our domain exhibits that are not well aligned with the requirements of machine-learning. These include: (i) a very high cost of errors; (ii) lack of training data; (iii) a semantic gap between results and their operational interpretation; (iv) enormous variability in input data; and (v) fundamental difficulties for conducting sound evaluation. While these challenges may not be surprising for those who have been working in the domain for some time, they can be easily lost on newcomers. To address them, we deem it crucial for any effective deployment to acquire deep, semantic insight into a system’s capabilities and limitations, rather than treating the system as a black box as unfortunately often seen.
此外，我们确定了我们的领域表现出的与机器学习要求不完全一致的进一步特征。 
其中包括： (i) 非常高的错误成本； (ii) 缺乏培训数据； (iii) 结果与其操作解释之间的语义差距； (iv) 输入数据的巨大差异； (v) 进行良好评价的根本困难。 
虽然这些挑战对于已经在该领域工作了一段时间的人来说可能并不奇怪，但对于新人来说，它们很容易迷失方向。 
为了解决这些问题，我们认为任何有效的部署都必须对系统的能力和局限性进行深入的语义洞察，而不是像不幸经常看到的那样将系统视为黑匣子。

We stress that we do not consider machine-learning an inappropriate tool for intrusion detection. 
Its use requires care, however: the more crisply one can define the context in which it operates, the better promise the results may hold.
Likewise, the better we understand the semantics of the detection process, the more operationally relevant the system will be. Consequently, we also present a set of guidelines meant to strengthen future intrusion detection research.
**我们强调，我们不认为机器学习是不适合入侵检测的工具**。 
然而，它的使用需要小心：越清晰地定义它的运行环境，结果就越有希望。
同样，我们越了解检测过程的语义，系统的操作相关性就越高。 
因此，我们还提出了一套旨在加强未来入侵检测研究的指导方针。

Throughout the discussion, we frame our mindset around on the goal of using an anomaly detection system effectively in the “real world”, i.e., in large-scale, operational environments. We focus on network intrusion detection as that is our main area of expertise, though we believe that similar arguments hold for host-based systems. For ease of exposition we will use the term anomaly detection somewhat narrowly to refer to detection approaches that rely primarily on machine-learning. By “machine-learning” we mean algorithms that are first trained with reference input to “learn”its specifics (either supervised or unsupervised), to then be deployed on previously unseen input for the actual detection process. While our terminology is deliberately a bit vague, we believe it captures what many in the field intuitively associate with the term “anomaly detection”.
在整个讨论过程中，我们围绕在“现实世界”（即大规模运营环境）中有效使用异常检测系统的目标构建我们的思维定势。 
我们专注于网络入侵检测，因为这是我们的主要专业领域，尽管我们相信类似的论点也适用于基于主机的系统。 
为了便于说明，我们将稍微狭义地使用术语异常检测来指代主要依赖机器学习的检测方法。 “机器学习”是指首先使用参考输入进行训练以“学习”其细节（有监督或无监督）的算法，然后将其部署在以前看不见的输入上以进行实际检测过程。 
虽然我们的术语故意有点模糊，但我们相信它捕捉到了该领域许多人直观地与“异常检测”一词相关的内容。

***

##  3. <a name='-1'></a>入侵检测中的机器学习
Anomaly detection systems find deviations from expected behavior. Based on a notion of normal activity, they report deviations from that profile as alerts. The basic assumption underlying any anomaly detection system—malicious activity exhibits characteristics not observed for normal usage—was first introduced by Denning in her seminal work on the host-based IDES system [9] in 1987. To capture normal activity, IDES (and its successor NIDES [10]) used a combination of statistical metrics and profiles. Since then, many more approaches have been pursued. Often, they borrow schemes from the machine learning community, such as information theory [11], neural networks [12], support vector machines [13], genetic algorithms [14], artificial immunesystems [15], and many more. In our discussion, we focus on anomaly detection systems that utilize such machine learning approaches.
异常检测系统发现与预期行为的偏差。 基于正常活动的概念，他们将与该配置文件的偏差报告为警报。 
任何异常检测系统的基本假设——恶意活动表现出正常使用中未观察到的特征——是由 Denning 在 1987 年她关于基于主机的 IDES 系统 [9] 的开创性工作中首次引入的。
为了捕获正常活动，IDES（及其 继任者 NIDES [10]）使用了统计指标和配置文件的组合。 
从那时起，人们开始寻求更多的方法。 
通常，他们从机器学习社区借用方案，例如信息论 [11]、神经网络 [12]、支持向量机 [13]、遗传算法 [14]、人工免疫系统 [15] 等等。 
在我们的讨论中，我们专注于利用这种机器学习方法的异常检测系统。

Chandola et al. provide a survey of anomaly detection in [16], including other areas where similar approaches are used, such as monitoring credit card spending patterns for fraudulent activity. While in such applications one is also looking for outliers, the data tends to be much more structured. For example, the space for representing credit card transactions is of relatively low dimensionality and semantically much more well-defined than network traffic [17].
Chandola等人。 在 [16] 中提供了对异常检测的调查，包括使用类似方法的其他领域，例如监控信用卡消费模式以发现欺诈活动。 
虽然在此类应用程序中，人们也在寻找异常值，但数据往往更加结构化。 
例如，用于表示信用卡交易的空间具有相对较低的维度，并且在语义上比网络流量[17]定义得更好。

Anomaly detection approaches must grapple with a set of well-recognized problems [18]: the detectors tend to generate numerous false positives; attack-free data for training is hard to find; and attackers can evade detection by gradually teaching a system to accept malicious activity as benign. Our discussion in this paper aims to develop a different general point: that much of the difficulty with anomaly detection systems stems from using tools borrowed from the machine learning community in inappropriate ways.
异常检测方法必须解决一系列公认的问题 [18]：检测器往往会产生大量误报； 很难找到用于训练的无攻击数据； 
攻击者可以通过逐渐教导系统接受恶意活动为良性来逃避检测。 
我们在本文中的讨论旨在提出一个不同的一般观点：异常检测系统的大部分困难源于以不适当的方式使用从机器学习社区借来的工具。

Compared to the extensive body of research, anomaly detection has not obtained much traction in the “real world”.
Those systems found in operational deployment are most commonly based on statistical profiles of heavily aggregated traffic, such as Arbor’s Peakflow [19] and Lanscope’s StealthWatch [20]. While highly helpful, such devices operate with a much more specific focus than with the generality that research papers often envision.2We see this situation as suggestive that many anomaly detection systems from the academic world do not live up to the requirements of operational settings.
与广泛的研究相比，异常检测在“现实世界”中并没有获得太多关注。
在运营部署中发现的那些系统通常基于大量聚合流量的统计配置文件，例如 Arbor 的 Peakflow [19] 和 Lanscope 的 StealthWatch [20]。 
虽然非常有用，但此类设备的运行重点比研究论文通常设想的普遍性要更加具体。
我们认为这种情况表明学术界的许多异常检测系统无法满足操作设置的要求。

***

##  4. <a name='-1'></a>使用机器学习的挑战

It can be surprising at first to realize that despite extensive academic research efforts on anomaly detection, the success of such systems in operational environments has been very limited. In other domains, the very same machine learning tools that form the basis of anomaly detection systems have proven to work with great success, and are regularly used in commercial settings where large quantities of data render manual inspection infeasible. We believe that this “success discrepancy” arises because the intrusion detection domain exhibits particular characteristics that make the effective deployment of machine learning approaches fundamentally harder than in many other contexts.
起初，令人惊讶的是，尽管在异常检测方面进行了广泛的学术研究，但此类系统在操作环境中的成功非常有限。 
在其他领域，构成异常检测系统基础的相同机器学习工具已被证明非常成功，并且经常用于大量数据导致手动检查不可行的商业环境中。 
我们认为，之所以出现这种“成功差异”，**是因为入侵检测领域表现出的特殊特征使得机器学习方法的有效部署从根本上比在许多其他情况下更难**。

In the following we identify these differences, with an aim of raising the community’s awareness of the unique challenges anomaly detection faces when operating on network traffic. 
==We note that our examples from other domains are primarily for illustration, as there is of course a continuous spectrum for many of the properties discussed (e.g., spam detection faces a similarly adversarial environment as intrusion detection does)==. 
We also note that we are network security researchers, not experts on machine-learning, and thus we argue mostly at an intuitive level rather than attempting to frame our statements in the formalisms employed for machine learning. However, based on discussions with colleagues who work with machine learning on a daily basis, we believe these intuitive arguments match well with what a more formal analysis would yield.
在下文中，我们确定了这些差异，目的是提高社区对异常检测在网络流量上运行时面临的独特挑战的认识。 
我们注意到，我们来自其他领域的示例主要是为了说明，因为讨论的许多属性当然有一个连续的频谱（例如，垃圾邮件检测面临与入侵检测类似的对抗环境）。 
我们还注意到，我们是网络安全研究人员，而不是机器学习方面的专家，因此我们主要在直觉层面进行争论，而不是试图以机器学习所采用的形式来构建我们的陈述。
然而，根据与每天从事机器学习工作的同事的讨论，我们认为这些直观的论点与更正式的分析所产生的结果非常吻合。

###  4.1. <a name='-1'></a>孤立点检测
Fundamentally, machine-learning algorithms excel much better at finding similarities than at identifying activity that does not belong there: the classic machine learning application is a classification problem, rather than discovering meaningful outliers as required by an anomaly detection system [21]. Consider product recommendation systems such as that used by Amazon [3]: it employs collaborative filtering, matching each of a user’s purchased (or positively rated) items with other similar products, where similarity is determined by products that tend be bought together. If the system instead operated like an anomaly detection system, it would look for items that are typically not bought together—a different kind of question with a much less clear answer, as according to [3], many product pairs have no common customers.
==从根本上说，机器学习算法在发现相似性方面比在识别不属于那里的活动方面表现得更好==：经典的机器学习应用程序是一个分类问题，而不是按照异常检测系统的要求发现有意义的异常值 [21]。 
考虑产品推荐系统，例如亚马逊 [3] 使用的产品：它采用协同过滤，将用户购买（或正面评价）的每个产品与其他类似产品进行匹配，其中相似性由倾向于一起购买的产品确定。 
如果系统改为像异常检测系统一样运行，它会寻找通常不一起购买的商品——这是一种不同类型的问题，答案不太清楚，根据 [3]，许多产品对没有共同的客户。

In some sense, outlier detection is also a classification problem: there are two classes, “normal” and “not normal”, and the objective is determining which of the two more likely matches an observation. However, a basic rule of machine-learning is that one needs to train a system with specimens of all classes, and, crucially, the number of representatives found in the training set for each class should be large [22]. Yet for anomaly detection aiming to find novel attacks, by definition one cannot train on the attacks of interest, but only on normal traffic, and thus having only one category to compare new activity against.
从某种意义上说，异常值检测也是一个分类问题：有两个类别，“正常”和“不正常”，目标是确定这两者中哪一个更可能与观察结果匹配。 
然而，**机器学习的一个基本规则是需要用所有类别的样本来训练一个系统，而且至关重要的是，在每个类别的训练集中找到的代表数量应该很大**[22]。 
==然而，对于旨在发现新攻击的异常检测，根据定义，不能训练感兴趣的攻击，而只能训练正常流量，因此只有一个类别可以比较新活动==。

In other words, one often winds up training an anomaly detection system with the opposite of what it is supposed to find—a setting certainly not ideal, as it requires having a perfect model of normality for any reliable decision. If, on the other hand, one had a classification problem with multiple alternatives to choose from, then it would suffice to have a model just crisp enough to separate the classes. To quote from Witten et al. [21]: The idea of specifying only positive examples and adopting a standing assumption that the rest are negative is called the closed world assumption.
. . . [The assumption] is not of much practical use in reallife problems because they rarely involve “closed” worlds in which you can be certain that all cases are covered.
换句话说，一个异常检测系统的训练结果往往与它应该找到的相反——这种设置肯定不理想，因为它需要一个完美的正常模型来做出任何可靠的决定。 
另一方面，如果有一个分类问题，有多种选择可供选择，那么有一个足够清晰的模型来区分类别就足够了。 
**引用 Witten 等人的观点。 [21]：仅指定正面示例并采用其余均为负面的常规假设的想法称为封闭世界假设**。在现实生活中的问题中并没有太大的实际用途，因为它们很少涉及“封闭”的世界，在这些世界中你可以确定所有的情况都被涵盖了。

Spam detection is an example from the security domain of successfully applying machine learning to a classification problem. Originally proposed by Graham [8], Bayesian frameworks trained with large corpora of both spam and ham have evolved into a standard tool for reliably identifying unsolicited mail.
The observation that machine learning works much better for such true classification problems then leads to the conclusion that anomaly detection is likely in fact better suited for finding variations of known attacks, rather than previously unknown malicious activity. In such settings, one can train the system with specimens of the attacks as they are known and with normal background traffic, and thus achieve a much more reliable decision process.
垃圾邮件检测是安全领域成功将机器学习应用于分类问题的一个例子。 
最初由 Graham [8] 提出，使用大量垃圾邮件和非垃圾邮件语料库训练的贝叶斯框架已经发展成为可靠识别垃圾邮件的标准工具。
观察到机器学习对此类真实分类问题的效果要好得多，**因此得出的结论是，异常检测实际上可能更适合发现已知攻击的变体，而不是以前未知的恶意活动**。 
在这种情况下，人们可以使用已知的攻击样本和正常的背景流量来训练系统，从而实现更可靠的决策过程。

###  4.2. <a name='-1'></a>错误的高成本
In intrusion detection, the relative cost of any misclassification is extremely high compared to many other machine learning applications. A false positive requires spending expensive analyst time examining the reported incident only to eventually determine that it reflects benign underlying activity. As argued by Axelsson, even a very small rate of false positives can quickly render an NIDS unusable [23].
False negatives, on the other hand, have the potential to cause serious damage to an organization: even a single compromised system can seriously undermine the integrity of the IT infrastructure. It is illuminating to compare such high costs with the impact of misclassifications in other domains:
在入侵检测中，与许多其他机器学习应用相比，任何错误分类的相对成本都非常高。 
误报需要花费大量分析师时间来检查报告的事件，最终确定它反映了良性的潜在活动。 
正如 Axelsson 所说，即使是非常小的误报率也会很快使 NIDS 无法使用 [23]。
另一方面，误报可能会对组织造成严重损害：即使是单个受损系统也可能严重破坏 IT 基础设施的完整性。 将如此高的成本与其他领域的错误分类的影响进行比较是很有启发性的：

####  4.2.1. <a name='-1'></a>产品推荐 
Product recommendation systems can readily tolerate errors as these do not have a direct negative impact.
While for the seller a good recommendation has the potential to increase sales, a bad choice rarely hurts beyond a lost opportunity to have made a more enticing recommendation. (In fact, one might imagine such systems deliberately making more unlikely guesses on occasion, with the hope of pointing customers to products they would not have otherwise considered.) If recommendations do not align well with the customers’interest, they will most likely just continue shopping, rather than take a damaging step such as switching to different seller. As Greg Linden said (author of the recommendation engine behind Amazon): “Recommendations involve a lot of guesswork. Our error rate will always be high.”
产品推荐系统很容易容忍错误，因为这些错误不会产生直接的负面影响。
虽然对于卖家来说，一个好的推荐有可能增加销售额，但一个糟糕的选择除了失去提出更诱人推荐的机会之外，很少会受到伤害。 
（事实上，人们可能会想象这样的系统有时会故意做出更不可能的猜测，希望将客户指向他们本来不会考虑的产品。）如果推荐与客户的兴趣不一致，他们很可能只是 继续购物，而不是采取破坏性的步骤，例如切换到不同的卖家。 
正如 Greg Linden（亚马逊背后的推荐引擎的作者）所说：“推荐涉及很多猜测。 我们的错误率总是很高。”

####  4.2.2. <a name='OCR'></a>OCR
OCR technology can likewise tolerate errors much more readily than an anomaly detection system. Spelling and grammar checkers are commonly employed to clean up results, weeding out the obvious mistakes. More generally, statistical language models associate probabilities with results, allowing for postprocessing of a system’s initial output [25]. In addition, users have been trained not to expected perfect documents but to proofread where accuracy is important. While this corresponds to verifying NIDS alerts manually, it is much quicker for a human eye to check spelling of a word than to validate a report of, say, a web server compromise. Similar to OCR, contemporary automated language translation operates at relatively large errors rates [7], and while recent progress has been impressive, nobody would expect more than a rough translation.
OCR 技术同样比异常检测系统更容易容忍错误。 
拼写和语法检查器通常用于清理结果，清除明显的错误。 
更一般地说，统计语言模型将概率与结果相关联，允许对系统的初始输出进行后处理 [25]。 此外，用户接受了培训，不要期望文档完美，而是要在准确性很重要的地方进行校对。 
虽然这对应于手动验证 NIDS 警报，但人眼检查单词的拼写比验证 Web 服务器入侵报告要快得多。 
与 OCR 类似，当代自动语言翻译的错误率相对较高 [7]，尽管最近的进展令人印象深刻，但没有人会期望粗略的翻译。

####  4.2.3. <a name='-1'></a>垃圾邮件检测
Spam detection faces a highly **unbalanced cost model**: false positives (i.e., ham declared as spam) can prove very expensive, but false negatives (spam not identified as such) do not have a significant impact. 
This discrepancy can allow for “lopsided” tuning, leading to systems that emphasize finding obvious spam fairly reliably, yet exhibiting less reliability for new variations hitherto unseen. 
For an anomaly detection system that primarily aims to find novel attacks, such performance on new variations rarely constitutes an appropriate trade-off.
垃圾邮件检测面临着一个高度不平衡的成本模型：误报（即，被声明为垃圾邮件的）可能被证明是非常昂贵的，但漏报（未识别为垃圾邮件）不会产生重大影响。 
这种差异可能会导致“不平衡”的调整，导致系统强调相当可靠地发现明显的垃圾邮件，但对迄今为止未见的新变化表现出较低的可靠性。 
对于主要旨在发现新攻击的异常检测系统，这种对新变体的性能很少构成适当的权衡。

Overall, an anomaly detection system faces a much more stringent limit on the number of errors that it can tolerate.
However, the intrusion detection-specific challenges that we discuss here all tend to increase error rates—even above the levels for other domains. We deem this unfortunate combination as the primary reason for the lack of success in operational settings.
总体而言，异常检测系统在其可以容忍的错误数量上面临着更为严格的限制。
然而，我们在这里讨论的特定于入侵检测的挑战都倾向于增加错误率——甚至高于其他领域的水平。 
我们认为这种不幸的组合是在操作环境中缺乏成功的主要原因。

###  4.3. <a name='-1'></a>语义鸿沟
Anomaly detection systems face a key challenge of transferring their results into actionable reports for the network operator. In many studies, we observe a lack of this crucial final step, which we term the semantic gap. Unfortunately, in the intrusion detection community we find a tendency to limit the evaluation of anomaly detection systems to an assessment of a system’s capability to reliably identify deviations from the normal profile. While doing so indeed comprises an important ingredient for a sound study, the next step then needs to interpret the results from an operator’s point of view—“What does it mean?”

Answering this question goes to the heart of the difference between finding “abnormal activity” and “attacks”. Those familiar with anomaly detection are usually the first to acknowledge that such systems are not targeting to identify malicious behavior but just report what has not been seen before, whether benign or not. We argue however that one cannot stop at that point. After all, the objective of deploying an intrusion detection system is to find attacks, and thus a detector that does not allow for bridging this gap is unlikely to meet operational expectations. The common experience with anomaly detection systems producing too many false positives supports this view: by definition, a machine learning algorithm does not make any mistakes within its model of normality; yet for the operator it is the results’ interpretation that matters.

When addressing the semantic gap, one consideration is the incorporation of local security policies. While often neglected in academic research, a fundamental observation about operational networks is the degree to which they differ: many security constraints are a site-specific property.
Activity that is fine in an academic setting can be banned in an enterprise network; and even inside a single organization, department policies can differ widely. Thus, it is crucial for a NIDS to accommodate such differences.

For an anomaly detection system, the natural strategy to address site-specifics is having the system “learn” them during training with normal traffic. However, one cannot simply assert this as the solution to the question of adapting to different sites; one needs to explicitly demonstrate it, since the core issue concerns that such variations can prove diverse and easy to overlook.

Unfortunately, more often than not security policies are not defined crisply on a technical level. For example, an environment might tolerate peer-to-peer traffic as long as it is not used for distributing inappropriate content, and that it remains “below the radar” in terms of volume. To report a violation of such a policy, the anomaly detection system would need to have a notion of what is deemed “appropriate” or “egregiously large” in that particular environment; a decision out of reach for any of today’s systems.

Reporting just the usage of P2P applications is likely not particularly useful, unless the environment flat-out bans such usage. In our experience, such vague guidelines are actually common in many environments, and sometimes originate in the imprecise legal language found in the “terms of service”to which users must agree [26].

The basic challenge with regard to the semantic gap is understanding how the features the anomaly detection system operates on relate to the semantics of the network environment. In particular, for any given choice of features there will be a fundamental limit to the kind of determinations a NIDS can develop from them. Returning to the P2P example, when examining only NetFlow records, it is hard to imagine how one might spot inappropriate content.3As another example, consider exfiltration of personally identifying information (PII). In many threat models, loss of PII ranks quite high, as it has the potential for causing major damage (either directly, in financial terms, or due to publicity or political fallout). On a technical level, some forms of PII are not that hard to describe; e.g., social security numbers as well bank account numbers follow specific schemes that one can verify automatically.4But an anomaly detection system developed in the absence of such descriptions has little hope of finding PII, and even given examples of PII and nonPII will likely have difficulty distilling rules for accurately distinguishing one from the other.
异常检测系统面临着将其结果转化为网络运营商可操作报告的关键挑战。
在许多研究中，我们观察到缺少这个关键的最后一步，我们称之为语义差距。
不幸的是，在入侵检测社区中，我们发现倾向于将异常检测系统的评估限制为评估系统可靠识别与正常配置文件的偏差的能力。
虽然这样做确实是一项健全研究的重要组成部分，但下一步需要从操作员的角度解释结果——“这是什么意思？

”回答这个问题是找到“异常”之间区别的核心活动”和“攻击”。
熟悉异常检测的人通常最先承认此类系统的目标不是识别恶意行为，而只是报告以前从未见过的情况，无论是良性与否。然而，我们认为不能在这一点上停下来。毕竟，部署入侵检测系统的目标是发现攻击，因此不允许弥合这一差距的检测器不太可能满足操作预期。异常检测系统产生过多误报的常见经验支持这一观点：根据定义，机器学习算法不会在其正常模型中犯任何错误；然而对于操作员来说，重要的是结果的解释。

在解决语义差距时，一个考虑因素是结合本地安全策略。虽然在学术研究中经常被忽视，但关于运营网络的一个基本观察是它们的不同程度：许多安全约束是特定于站点的属性。
可以在企业网络中禁止在学术环境中进行的活动；甚至在单个组织内部，部门政策也可能大相径庭。因此，对于 NIDS 来说，适应这些差异至关重要。

对于异常检测系统，解决站点特定问题的自然策略是让系统在正常流量的训练期间“学习”它们。但是，不能简单地将其断言为适应不同站点问题的解决方案；需要明确地证明这一点，因为核心问题涉及到这种变化可能被证明是多种多样的并且容易被忽视。

不幸的是，安全策略通常没有在技术层面上明确定义。例如，一个环境可能会容忍点对点流量，只要它不用于分发不适当的内容，并且就数量而言它仍然“不为人知”。为了报告违反此类政策的行为，异常检测系统需要了解在该特定环境中什么被认为是“适当的”或“异常大的”；当今任何系统都无法做出决定。
仅报告 P2P 应用程序的使用可能不是特别有用，除非环境完全禁止此类使用。根据我们的经验，这种模糊的指导方针实际上在许多环境中都很常见，并且有时源于用户必须同意的“服务条款”中不精确的法律语言[26]。

关于语义差距的基本挑战是理解异常检测系统操作的特征如何与网络环境的语义相关。特别是，对于任何给定的特征选择，NIDS 可以从这些特征中做出的决定类型都会受到基本限制。回到 P2P 示例，仅检查 NetFlow 记录时，很难想象如何发现不适当的内容。3 作为另一个示例，请考虑个人识别信息 (PII) 的泄露。在许多威胁模型中，PII 丢失的排名很高，因为它有可能造成重大损害（直接、财务方面，或由于宣传或政治影响）。在技​​术层面上，某些形式的 PII 并不难描述。例如，社会保险号和银行帐号遵循可以自动验证的特定方案。4 但是在没有此类描述的情况下开发的异常检测系统几乎没有希望找到 PII，即使给出 PII 和非 PII 的示例也可能会遇到困难用于准确区分一种与另一种的提炼规则。

###  4.4. <a name='-1'></a>网络流量的多样性
Network traffic often exhibits much more diversity than people intuitively expect, which leads to misconceptions about what anomaly detection technology can realistically achieve in operational environments. Even within a single network, the network’s most basic characteristics—such as bandwidth, duration of connections, and application mix—can exhibit immense variability, rendering them unpredictable over short time intervals (seconds to hours). The widespread prevalence of strong correlations and “heavytailed” data transfers [32], [33] regularly leads to large bursts of activity. It is crucial to acknowledge that in networking such variability occurs regularly; it does not represent anything unusual. For an anomaly detection system, however, such variability can prove hard to deal with, as it makes it difficult to find a stable notion of “normality”.

One way to reduce the diversity of Internet traffic is to employ aggregation. While highly variable over smallto-medium time intervals, traffic properties tend to greater stability when observed over longer time periods (hours to days, sometimes weeks). For example, in most networks time-of-day and day-of-week effects exhibit reliable patterns: if during today’s lunch break, the traffic volume is twice as large as during the corresponding time slots last week, that likely reflects something unusual occurring. Not coincidentally, one form of anomaly detection system we do find in operation deployment is those that operate on highly aggregated information, such as “volume per hour” or “connections per source”. On the other hand, incidents found by these systems tend to be rather noisy anyway—and often straight-forward to find with other approaches (e.g., simple threshold schemes). This last observation goes to the heart of what can often undermine anomaly detection research efforts: a failure to examine whether simpler, non-machine learning approaches might work equally well.

Finally, we note that traffic diversity is not restricted to packet-level features, but extends to application-layer information as well, both in terms of syntactic and semantic variability. Syntactically, protocol specifications often purposefully leave room for interpretation, and in heterogeneous traffic streams there is ample opportunity for corner-case situations to manifest (see the discussion of “crud” in [34]).
Semantically, features derived from application protocols can be just as fluctuating as network-layer packets (see, e.g., [35], [36]).
网络流量通常表现出比人们直观预期的更多的多样性，这导致人们对异常检测技术在操作环境中可以实际实现的目标产生误解。即使在单个网络中，==网络的最基本特征（例如带宽、连接持续时间和应用程序组合）也可能表现出巨大的可变性==，使它们在很短的时间间隔（几秒到几小时）内变得不可预测。
强相关性和“重尾”数据传输的广​​泛流行 [32]、[33] 经常导致大量活动。承认在网络中经常发生这种变化是至关重要的；它并不代表任何异常。然而，对于异常检测系统，这种可变性很难处理，因为它很难找到一个稳定的“正常”概念。

减少 Internet 流量多样性的一种方法是采用聚合。虽然在中小时间间隔内变化很大，但在较长时间段（数小时到数天，有时是数周）观察时，交通属性往往会更加稳定。例如，在大多数网络中，一天中的时间和一周中的一天的影响表现出可靠的模式：如果在今天的午休时间，流量是上周相应时间段的两倍，这可能反映了一些不寻常的事情发生.并非巧合的是，我们在操作部署中发现的一种异常检测系统是那些对高度聚合的信息进行操作的系统，例如“每小时量”或“每个源的连接数”。另一方面，无论如何，这些系统发现的事件往往相当嘈杂——而且通常可以直接用其他方法（例如，简单的阈值方案）找到。最后一个观察结果触及了通常会破坏异常检测研究工作的核心：未能检查更简单的非机器学习方法是否同样有效。

最后，我们注意到流量多样性不仅限于数据包级特征，还扩展到应用层信息，包括句法和语义可变性。从句法上讲，协议规范通常有目的地为解释留出空间，并且在异构流量流中，极端情况有足够的机会表现出来（参见 [34] 中关于“crud”的讨论）。
从语义上讲，从应用协议派生的特征可以像网络层数据包一样波动（参见，例如，[35]、[36]）。

###  4.5. <a name='-1'></a>评估的困难
For an anomaly detection system, a thorough evaluation is particularly crucial to perform, as experience shows that many promising approaches turn out in practice to fall short of one’s expectations. That said, devising sound evaluation schemes is not easy, and in fact turns out to be more difficult than building the detector itself. Due to the opacity of the detection process, the results of an anomaly detection system are harder to predict than for a misuse detector. We discuss evaluation challenges in terms of the difficulties for (i) finding the right data, and then (ii) interpreting results.
对于异常检测系统，进行彻底的评估尤其重要，因为经验表明，许多有前途的方法在实践中都达不到预期。 也就是说，设计合理的评估方案并不容易，实际上比构建探测器本身更困难。 
由于检测过程的不透明性，异常检测系统的结果比误用检测器更难预测。 
我们从以下几个方面讨论评估挑战：（i）找到正确的数据，然后（ii）解释结果。

####  4.5.1. <a name='DifficultiesofData'></a>Difficulties of Data
Arguably the most significant challenge an evaluation faces is the lack of appropriate public datasets for assessing anomaly detection systems. In other domains, we often find either standardized test suites available, or the possibility to collect an appropriate corpus, or both. For example, for automatic language translation “a large training set of the input-output behavior that we seek to automate is available to us in the wild” [37]. For spam detectors, dedicated “spam feeds” [38] provide large collections of spam free of privacy concerns. Getting suitable collections of “ham” is more difficult, however even a small number of private mail archives can already yield a large corpus [39]. For OCR, sophisticated methods have been devised to generate ground-truth automatically [40]. In our domain, however, we often have neither standardized test sets, nor any appropriate, readily available data.

The two publicly available datasets that have provided something of a standardized setting in the past—the DARPA/Lincoln Labs packet traces [41], [42] and the KDD Cup dataset derived from them [43]—are now a decade old, and no longer adequate for any current study. The DARPA dataset contains multiple weeks of network activity from a simulated Air Force network, generated in 1998 and refined in 1999. Not only is this data synthetic, and no longer even close to reflecting contemporary attacks, but it also has been so extensively studied over the years that most members of the intrusion detection community deem it wholly uninteresting if a NIDS now reliably detects the attacks it contains.
(Indeed, the DARPA data faced pointed criticisms not long after its release [44], particularly regarding the degree to which simulated data can be appropriate for the evaluation of a NIDS.) The KDD dataset represents a distillation of the DARPA traces into features for machine learning. Not only does it inherit the shortcomings of the DARPA data, but the features have also turned out to exhibit unfortunate artifacts [45].

Given the lack of publicly available data, it is natural to ask why we find such a striking gap in our community.5The primary reason clearly arises from the data’s sensitive nature: the inspection of network traffic can reveal highly sensitive information, including confidential or personal communications, an organization’s business secrets, or its users’network access patterns. Any breach of such information can prove catastrophic not only for the organization itself, but also for affected third parties. It is understandable that in the face of such high risks, researchers frequently encounter insurmountable organizational and legal barriers when they attempt to provide datasets to the community.

Given this difficulty, researchers have pursued two alternative routes in the past: simulation and anonymization.
As demonstrated by the DARPA dataset, network traffic generated by simulation can have the major benefit of being free of sensitivity concerns. However, Internet traffic is already exceedingly difficult to simulate realistically by itself [48]. Evaluating an anomaly detection system that strives to find novel attacks using only simulated activity will often lack any plausible degree of realism or relevance.

One can also sanitize captured data by, e.g., removing or anonymizing potentially sensitive information [49], [50], [51]. However, despite intensive efforts [52], [53], publishing such datasets has garnered little traction to date, mostly one suspects for the fear that information can still leak. (As [54] demonstrates, this fear is well justified.) Furthermore, even if a scrubbed dataset is available, its use with an anomaly detection system can be quite problematic, since by definition such systems look precisely for the kind of artifacts that tend to be removed during the anonymization process [55].

Due to the lack of public data, researchers are forced to assemble their own datasets. However, in general this is not an easy task, as most lack access to appropriately sized networks. It is crucial to realize that activity found in a small laboratory network differs fundamentally from the aggregate traffic seen upstream where NIDSs are commonly deployed [26]. Conclusions drawn from analyzing a small environment cannot be generalized to settings of larger scale.

There is unfortunately no general answer to countering the lack of data for evaluation purposes. For any study it is thus crucial to (i) acknowledge shortcomings that one’s evaluation dataset might impose, and (ii) consider alternatives specific to the particular setting. We return to these points in Section IV-D1.
可以说，评估面临的最重大挑战是缺乏用于评估异常检测系统的适当公共数据集。在其他领域，我们经常发现可用的标准化测试套件，或收集适当语料库的可能性，或两者兼而有之。例如，对于自动语言翻译，“我们寻求自动化的输入输出行为的大量训练集可供我们在野外使用”[37]。对于垃圾邮件检测器，专用的“垃圾邮件提要”[38] 提供大量垃圾邮件，无需担心隐私问题。获得合适的“火腿”集合更加困难，但是即使是少量的私人邮件档案也已经可以产生大量的语料库[39]。对于 OCR，已经设计了复杂的方法来自动生成地面实况 [40]。然而，在我们的领域中，我们通常既没有标准化的测试集，也没有任何合适的、现成的数据。

过去提供了某种标准化设置的两个公开可用的数据集——DARPA/Lincoln Labs 数据包跟踪 [41]、[42] 和从中派生的 KDD Cup 数据集 [43]——现在已有十年历史，并且不再适合任何当前的研究。 
DARPA 数据集包含来自模拟空军网络的数周网络活动，该网络于 1998 年生成并于 1999 年完善。
这些数据不仅是合成的，甚至不再接近反映当代攻击，而且它也已被广泛研究如果 NIDS 现在可靠地检测到它包含的攻击，入侵检测社区的大多数成员都认为这完全无趣。
（事实上​​，DARPA 数据在发布后不久就遭到了尖锐的批评 [44]，尤其是关于模拟数据在多大程度上适合评估 NIDS。）机器学习。它不仅继承了 DARPA 数据的缺点，而且这些特征也显示出令人遗憾的伪像 [45]。

鉴于缺乏公开可用的数据，很自然地要问为什么我们发现我们的社区存在如此显着的差距。 5 主要原因显然来自数据的敏感性：对网络流量的检查可以揭示高度敏感的信息，包括机密或个人信息。通信、组织的商业机密或其用户的网络访问模式。任何违反此类信息的行为不仅对组织本身，而且对受影响的第三方都可能是灾难性的。可以理解的是，面对如此高的风险，研究人员在尝试向社区提供数据集时经常会遇到难以逾越的组织和法律障碍。

鉴于这一困难，研究人员过去曾采用两条替代路线：模拟和匿名化。
正如 DARPA 数据集所证明的那样，模拟生成的网络流量的主要好处是没有敏感问题。然而，互联网流量本身已经非常难以真实地模拟[48]。评估试图仅使用模拟活动来发现新攻击的异常检测系统通常会缺乏任何合理程度的真实性或相关性。

人们还可以通过例如删除或匿名化潜在敏感信息来清理捕获的数据 [49]、[50]、[51]。然而，尽管付出了巨大的努力[52]，[53]，迄今为止发布此类数据集并没有引起多大的关注，主要是因为担心信息仍然会泄露而受到怀疑。 （正如 [54] 所证明的，这种担心是有充分理由的。）此外，即使有一个清理过的数据集可用，它与异常检测系统的使用也可能存在很大问题，因为根据定义，这些系统会精确地寻找倾向于在匿名化过程中被删除 [55]。

由于缺乏公共数据，研究人员被迫组装自己的数据集。然而，总的来说，这并不是一件容易的事，因为大多数人都无法访问适当规模的网络。认识到在小型实验室网络中发现的活动与通常部署 NIDS 的上游总流量根本不同，这一点至关重要 [26]。分析小环境得出的结论不能推广到更大规模的环境。

不幸的是，没有针对评估目的缺乏数据的一般性答案。因此，对于任何研究来说，（i）承认一个人的评估数据集可能带来的缺点，以及（ii）考虑特定环境的替代方案都是至关重要的。我们在第 IV-D1 节中回到这些点。

####  4.5.2. <a name='MindtheGap:'></a>Mind the Gap:
The semantic gap requires any study to perform an explicit final step that tends to be implicit in other domains: changing perspective to that of a user of the system. In addition to correctly identifying attacks, an anomaly detection system also needs to support the operator in understanding the activity and enabling a quick assessment of its impact. Suppose a system correctly finds a previously unknown web server exploit, yet only reports it as “HTTP traffic of host did not match the normal profile”.
The operator will spend significant additional effort figuring out what happened, even if already having sufficient trust in the system to take its alerts seriously. In other applications of machine learning, we do not see a comparable problem, as results tend to be intuitive there. Returning to spam detection again, if the detector reports a mail as spam, there is not much room for interpretation left.

We argue that when evaluating an anomaly detection system, understanding the system’s semantic properties—the operationally relevant activity that it can detect, as well as the blind spots every system will necessarily have—is much more valuable than identifying a concrete set of parameters for which the system happens to work best for a particular input. The specifics of network environments differ too widely to allow for predicting performance in other settings based on just numbers. Yet, with insight into the conceptual capabilities of a system, a network operator can judge a detector’s potential to support different operational concerns as required. That said, we note that Tan et al. demonstrated the amount of effort it can require to understand a single parameter’s impact, even with an conceptually simple anomaly detection system [56].
语义鸿沟要求任何研究执行一个在其他领域往往隐含的明确的最后一步：将视角转变为系统用户的视角。除了正确识别攻击外，异常检测系统还需要支持操作员了解活动并快速评估其影响。假设系统正确地发现了一个以前未知的 Web 服务器漏洞，但仅将其报告为“主机的 HTTP 流量与正常配置文件不匹配”。
操作员将花费大量额外的精力来弄清楚发生了什么，即使已经对系统有足够的信任以认真对待其警报。在机器学习的其他应用中，我们没有看到类似的问题，因为那里的结果往往是直观的。再次回到垃圾邮件检测，如果检测器将邮件报告为垃圾邮件，则没有太多解释空间。

我们认为，在评估异常检测系统时，了解系统的语义属性——它可以检测到的与操作相关的活动，以及每个系统必然具有的盲点——比识别一组具体的参数更有价值。系统碰巧最适合特定输入。网络环境的细节差异太大，无法仅根据数字预测其他设置中的性能。然而，通过深入了解系统的概念能力，网络运营商可以根据需要判断探测器支持不同运营问题的潜力。也就是说，我们注意到 Tan 等人。证明了即使使用概念上简单的异常检测系统[56]，也可以理解单个参数的影响所需的工作量。

####  4.5.3. <a name='AdversarialSetting:'></a>Adversarial Setting:
A final characteristic unique to the intrusion detection domain concerns the adversarial environment such systems operate in. In contrast, users of OCR systems won’t try to conceal characters in the input, nor will Amazon customers have much incentive (or opportunity) to mislead the company’s recommendation system. Network intrusion detection, however, must grapple with a classic arms-race: attackers and defenders each improve their tools in response to the other side devising new techniques. One particular, serious concern in this regard is evasion: attackers adjusting their activity to avoid detection. While evasion poses a fundamentally hard problem for any NIDS [57], anomaly detection faces further risks due to the nature of underlying machine learning. In [58], Fogla and Lee present an automated approach to mutate attacks so that they match a system’s normal profile. More generally, in [59] Barreno et al. present a taxonomy of attacks on machinelearning systems.

From a research perspective, addressing evasion is a stimulating topic to explore; on theoretical grounds it is what separates intrusion detection most clearly from other domains. However, we argue that from a practical perspective, the impact of the adversarial setting is not necessarily as significant as one might initially believe. Exploiting the specifics of a machine learning implementation requires significant effort, time, and expertise on the attacker’s side.
Considering that most of today’s attacks are however not deliberately targeting handpicked victims—yet simply exploit whatever sites they find vulnerable, indiscriminantly seeking targets— the risk of an anomaly detector falling victim to a sophisticated evasion attack is small in many environments. Assuming such a threat model, it appears prudent to focus first on addressing the many other challenges in using machine learning effectively, as they affect a system’s operational performance more severely.
入侵检测领域特有的最后一个特征涉及此类系统运行的对抗环境。相比之下，OCR 系统的用户不会试图隐藏输入中的字符，亚马逊客户也不会有太多的动机（或机会）来误导公司推荐系统。然而，网络入侵检测必须应对经典的军备竞赛：攻击者和防御者各自改进他们的工具，以响应对方设计新技术。在这方面一个特别严重的问题是规避：攻击者调整他们的活动以避免被发现。虽然规避对任何 NIDS [57] 来说都是一个根本性的难题，但由于基础机器学习的性质，异常检测面临着进一步的风险。在 [58] 中，Fogla 和 Lee 提出了一种自动变异攻击的方法，以便它们匹配系统的正常配置文件。更一般地，在 [59] Barreno 等人中。提出了对机器学习系统的攻击分类。

从研究的角度来看，解决逃避问题是一个令人兴奋的探索话题。从理论上讲，这是入侵检测与其他领域最明显的区别。然而，我们认为，从实践的角度来看，对抗环境的影响并不一定像人们最初认为的那么重要。利用机器学习实现的细节需要攻击者付出大量的努力、时间和专业知识。
然而，考虑到今天的大多数攻击并不是故意针对精心挑选的受害者——而是简单地利用他们发现的任何易受攻击的站点，不加选择地寻找目标——在许多环境中，异常检测器成为复杂规避攻击受害者的风险很小。假设有这样一个威胁模型，首先专注于解决有效使用机器学习的许多其他挑战似乎是明智的，因为它们会更严重地影响系统的操作性能。

##  5. <a name='-1'></a>使用机器学习的建议
In light of the points developed above, we now formulate guidelines that we hope will help to strengthen future research on anomaly detection. We note that we view these guidelines as touchstones rather than as firm rules; there is certainly room for further discussion within the wider intrusion detection community.

###  5.1. <a name='UnderstandingtheThreatModel'></a>Understanding the Threat Model

###  5.2. <a name='KeepingTheScopeNarrow'></a>Keeping The Scope Narrow

###  5.3. <a name='ReducingtheCosts'></a>Reducing the Costs

###  5.4. <a name='Evaluation'></a>Evaluation

##  6. <a name='-1'></a>结论
Our work examines the surprising imbalance between the extensive amount of research on machine learning-based anomaly detection pursued in the academic intrusion detection community, versus the lack of operational deployments of such systems. We argue that this discrepancy stems in large part from specifics of the problem domain that make it significantly harder to apply machine learning effectively than in many other areas of computer science where such schemes are used with greater success. The domain-specific challenges include: (i) the need for outlier detection, while machine learning instead performs better at finding similarities; (ii) very high costs of classification errors, which render error rates as encountered in other domains unrealistic; (iii) a semantic gap between detection results and their operational interpretation; (iv) the enormous variability of benign traffic, making it difficult to find stable notions of normality; (v) significant challenges with performing sound evaluation; and (vi) the need to operate in an adversarial setting. While none of these render machine learning an inappropriate tool for intrusion detection, we deem their unfortunate combination in this domain as a primary reason for its lack of success.
To overcome these challenges, we provide a set of guidelines for applying machine learning to network intrusion detection. In particular, we argue for the importance of obtaining insight into the operation of an anomaly detection system in terms of its capabilities and limitations from an operational point of view. It is crucial to acknowledge that the nature of the domain is such that one can always find schemes that yield marginally better ROC curves than anything else has for a specific given setting. Such results however do not contribute to the progress of the field without any semantic understanding of the gain.
We hope for this discussion to contribute to strengthening future research on anomaly detection by pinpointing the fundamental challenges it faces. We stress that we do not consider our discussion as final, and we look forward to the intrusion detection community engaging in an ongoing dialog on this topic.
我们的工作检查了学术入侵检测社区对基于机器学习的异常检测的大量研究与缺乏此类系统的操作部署之间的惊人不平衡。
我们认为，这种差异在很大程度上源于问题领域的具体情况，这使得有效地应用机器学习比在计算机科学的许多其他领域取得更大成功的计算机科学领域更加困难。
特定领域的挑战包括：（i）需要异常值检测，而机器学习在发现相似性方面表现更好； (ii) 分类错误的成本非常高，这使得在其他领域遇到的错误率变得不切实际； (iii) 检测结果与其操作解释之间的语义差距； (iv) 良性交通的巨大可变性，难以找到稳定的常态概念； ㈤ 进行合理评价的重大挑战； (vi) 在对抗环境中运作的需要。虽然这些都没有使机器学习成为入侵检测的不合适工具，但我们认为它们在该领域的不幸结合是其缺乏成功的主要原因。
为了克服这些挑战，我们提供了一套将机器学习应用于网络入侵检测的指南。
特别是，我们主张从操作的角度了解异常检测系统的操作的能力和局限性的重要性。
重要的是要承认域的性质是这样的，即对于特定的给定设置，人们总是可以找到产生比其他任何方法都更好的 ROC 曲线的方案。
然而，如果没有对增益的任何语义理解，这样的结果不会有助于该领域的进步。
我们希望通过确定异常检测面临的基本挑战，本次讨论有助于加强对异常检测的未来研究。
我们强调，我们不认为我们的讨论是最终的，我们期待入侵检测社区参与有关该主题的持续对话。
