Title: 机器学习系统的设计与调优
Date: 2015-12-02 23:08
Modified: 2015-12-02 23:08
Slug: evaluating-2
Tags: machine-learning
Authors: Joey Huang
Summary: 本文以设计一个垃圾邮件过滤系统为例，谈谈如何设计一个机器学习系统。同时介绍查准率，召回率以及 F1Score 来评价算法的性能。


## 构建垃圾邮件过滤系统

**特征选择**

在实践中，可以遍历所有的训练数据集，即所有的垃圾邮件和所有的非垃圾邮件，找出出现频率最高的 10,000 - 50,000 个单词作为特征，假设特征数量记为 n。这样**一封邮件就可以用一个 n 维向量来表示**，即 n 个特征单词是否出现在邮件里，如果出现记为 1 不出现记为 0 。

**构建步骤**

* 收集尽量多的数据，如 [honeypot][1] 项目
* 从邮件路由信息中提取出有效的特征来区分垃圾邮件，路由信息放在邮件头部
* 从邮件的内容中提取复杂特征
* 开发一套算法来检查拼写错误。因为很多算法从邮件内容中通过关键字为特征来区分垃圾邮件，垃圾邮件系统为了跳过这个检查，故意把一些敏感词拼错，这样规避垃圾邮件检查机制

至于哪个方法是最有效的，需要头脑风暴或者事先详细研究才能得出结论。当然，在算法通过检验之前，很难事先判断哪个特征是最有效的。

## 错误分析

用机器学习算法解决问题时，可以偿试如下的策略

* **从简单的算法开始**，先实现出来，然后使用交叉验证数据来验证结果。
* **画出学习曲线**，诊断算法的问题和优化方向，是需要去获取更多训练数据还是要增加特征等。
* 错误分析：**针对交叉验证数据的错误项进行手动分析**，试图从这些错误结果里找出更多线索和特征。

**错误分析实例**

假设我们实现的垃圾邮件过滤算法，针对 500 封交叉验证数据里有 100 封被错误分类了，那么我们可以进行

1. 手动检查这些被错误分类的邮件类型，比如钓鱼邮件，卖药的邮件等等，通过手动分析总结出哪种类型的邮件被错误地分类数量最多，然后先把精力花在这种类型的邮件上面。
2. 有哪些线索或特征有助于算法正确鉴别这些邮件。比如通过分析，我们发现异常路由的邮件数量有多少，错误拼写的邮件有多少，异常标点符号的邮件有多少。通过总结这些特征，决定我们应该要把时间花在哪方面来改善算法性能。

比如，我们在实现垃圾邮件鉴别算法时，我们需要决定 Dicount/Discounts/Discounted/Discouting 等单词视为同一个单词还是不同的单词。如果要视为相同的单词，可以使用词干提取法 (Porter Stemmer) ，但使用词干提取法一样会带来问题，比如会错误地把 universe/university 归类为同一个单词。这个时候如何决策呢？

一个可行的办法是分别计算使用了词干提取法和不使用时候的交叉验证数据集成本 $J_{cv}(\theta)$ 和测试数据集成本 $J_{test}(\theta)$ ，这样来判断到底是使用更好还是不使用性能更好。

实际上，优化算法过程中的很多偿试都可以使用这个方法来判断是否是有效的优化策略。

## 处理有倾向性的数据

比如针对癌症筛查算法，根据统计，普通肿瘤中癌症的概率是 0.5% 。我们有个机器学习算法，在交叉验证数据时得出的准确率是 99.2%，错误率是 0.8% 。这个算法到底是好还是坏呢？如果努力改进算法，最终在交叉验证数据集上得出的准确率是 99.5%，错误率是 0.5% 到底算法性能是提高了还是降低了呢？

坦白讲，如果单纯从交叉验证数据集上测试准确率的方法很难进行判断到底算法是变好了还是变坏了。因为这个事情的先验概率太低了，假如我们写了一个超级简单的预测函数，总是返回 0，即总是认为不会得癌症，那么我们这个超级简单的预测函数在交叉验证数据集上得到的准确率是 99.5%，错误率是 0.5% 。因为总体而言，只有那 0.5% 真正得癌症的可怜虫被我们误判了。

那么我们怎么样来衡量分类问题的准确性能呢？我们引入了另外两个概念，**查准率 (Precision)** 和 **召回率 (Recall)**。还是以癌症筛查为例：

预测数据/实际数据  | 实际恶性肿瘤    | 实际良性肿瘤
-----------------|----------------|--------------
预测恶性肿瘤      | TruePositive   | FalsePositive
预测良性肿瘤      | FalseNegative  | TrueNegative


$$
Precision = \frac{TruePosition}{TruePosition + FalsePositive}
$$

$$
Recall = \frac{TruePositive}{TruePositive + FalseNegative}
$$

在处理先验概率低的问题时，我们总是把概率较低的事件定义为 1 ，并且总是把 $y=1$ 作为 Positive 的预测结果。有了这个公式，如果一个简单地返回 0 的预测函数，那么它的查准率和召回率都为 0。这显然不是个好的预测模型。

**TIPS**

如何理解 `True/False` 和 `Positive/Negative` ？`True/False` 表示预测结果是否正确，而 `Positive/Negative` 表示预测结果是 1 (恶性肿瘤) 或 0 (良性肿瘤)。故，TruePositive 表示正确地预测出恶性肿瘤的数量；FalsePositive 表示错误地预测出恶性肿瘤的数量；FalseNegative 表示错误地预测出良性肿瘤的数量。


## 在查准率和召回率之间权衡

假设我们想提高癌症的查准率，即只有在很有把握的情况下才预测为癌症。回忆我们在逻辑回归算法里，当 $h_\theta(x) >= 0.5$ 时，我们就预测 $y = 1$ ，为了提高查准率，可以把门限值从 0.5 提高到 0.8 之类的。这样就提高了查准率，但这样会降低召回率。同样的道理，我们如果想提高召回率，可以降低门限值，从 0.5 降到 0.3 。这样召回率就会提高，但查准率就会降低。所以在实际问题时，可以要接实际问题，去判断是查准率重要还是召回率重要，根据重要性去调整门限值。

**如何评价算法的好坏**

由于我们现在有两个指标，查准率和如回率，如果有一个算法的查准率是 0.5, 召回率是 0.4；另外一个算法查准率是 0.02, 召回率是 1.0；那么两个算法到底哪个好呢？

为了解决这个问题，我们引入了 $F_1Score$ 的概念

$$
F_1Score = 2 \frac{PR}{P + R}
$$

其中 P 是查准率，R 是召回率。这样就可以用一个数值直接判断哪个算法性能更好。典型地，如果查准率或召回率有一个为 0，那么 $F_1Score$ 将会为 0。而理想的情况下，查准率和召回率都为 1 ，则算出来的 $F_1Score$ 为 1。这是最理想的情况。

**自动选择门限值**

前文介绍过，门限值可以调节查准率和召回率的高低。那么如何自动选择门限值以便让算法的性能最优呢？我们可以使用交叉验证数据，算出使 $F_1Score$ 最大的门限值。这个值就是我们自动选择出来的最优的门限值。

## 使用大量的数据集

Michele Banko and Eric Brill 在 2011 年用四种算法进行了一个自然语言的机器学习训练，结果发现，数据量越大，训练出来的算法准确性越高。他们得出了下图的结论。

![Accuracy and data size](http://img.ptcms.csdn.net/article/201506/18/55828e0dad1e5.jpg)

然后这个结论是些前提：**有足够的特征进行机器学习**。怎么样判断是否有足够的特征呢？我们可以让这个领域的专家来人工预测。比如给出一个房子的面积，让房产经纪人预测其房价，他肯定无法正确地预测。因为特征不足，很难只根据房子的面积推算出房子的价格。

怎么样从理论上证明这个结论呢？我们知道，如果我们有足够的特征来进行预测，意味着我们可以构建足够复杂的模型（比如神经网络）来让我们的预测函数有比较低的偏差 (Low Bais)，即让训练数据集成本 $J_{train}(\theta)$ 的值很小。如果我们有足够多的数据，就可以确保我们可以训练出一个低方差 (Low Variance) 的算法，即我们可以让交叉验证数据集成本 $J_{cv}(\theta)$ 接近训练数据集成本 $J_{train}(\theta)$ 。这样最终我们的测试数据集成本  $J_{test}(\theta)$ 也会靠近训练数据集成本 $J_{train}(\theta)$ 。

[1]: http://www.projecthoneypot.org