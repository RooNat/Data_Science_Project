# Project Plan

[toc]

## Abstract

> A paragraph or two that briefly outlines the motivation of your work, what you will do, and a "fairy tale" outcome of what you hope to find. 

随着人们对于加密货币的兴趣爆发式增长，预测加密货币的价格变化也成了金融市场关注的问题。然而，加密货币以极强的波动性为特点，因此较难预测。近些年已经逐渐有研究证明加密货币的价格会受到社交媒体公众情绪的影响。受此启发，本研究目的是探寻多源社交媒体平台的公众情绪与主流加密货币，比特币，的价格的内在联系以及公众情绪对价格变动的滞后影响。并致力于使用公众情绪作为预测比特币价格的特征，构建有效的价格回报预测模型、价格方向预测模型和投资者行为推荐模型，以实现利润最大化。

In this paper， the study 从公开数据集获取了2022年上半年的每小时推文、谷歌趋势数据、及Reddit posts，此外，使用了时间序列预测技术和情感分析模型构建 feature set，并将LSTM模型作为主要的预测模型。研究发现，公众情绪的确对加密货币价格产生了影响并且可以作为其有效的预测指标，不同的社交媒体来源对加密货币的影响并不一致。研究结果揭示了谷歌数据对比特币价格回报和价格变动方向的滞后影响，以及推文情绪在短期内对价格变动方向的影响。通过使用社交媒体数据构建特征，比特币价格方向的预测准确率得到了显著提升，其中，在短期内，推文情绪的影响力可使得价格变动预测准确率提高了3%至8%，在长期内，推文情绪得分与谷歌趋势相结合作为特征集时，仍有效地提高了0.6%的平均准确率。最后，基于社交媒体情绪构建行为推荐模型，该模型可以根据历史价格为投资者提供接下来的行为建议，例如"买入"，“卖出” 和 “等待”。通过调整模型阈值，可使一个月内的最大价格回报率达到21.23%。

> With the explosion of interest in cryptocurrencies, predicting price changes in cryptocurrencies has become a concern for the financial markets. However, cryptocurrencies are characterised by extreme volatility and are therefore difficult to predict. In recent years, there has been a growing body of research demonstrating that cryptocurrency prices are influenced by public sentiment on social media. Inspired by this, the aim of this study is to explore and visualise the relationship between public sentiment and the prices of different cryptocurrencies across multiple sources of social media platforms. In this paper, the study crawled cryptocurrency-related posts and messages from Twitter, Google, Reddit and online forums, in addition to using time series and sentiment analysis models to construct an effective price direction prediction model and investor behaviour recommendation model to maximise profits. prediction techniques and sentiment analysis models to construct feature sets, and used LSTM-GRU stacking models as the main prediction models. It was found that public sentiment does have an impact on cryptocurrency prices and can be an effective predictor, that the impact of different social media sources on cryptocurrencies is inconsistent, and that by using social media data to construct features, the accuracy of predicting the direction of cryptocurrency prices is significantly improved. Finally, a behavioural recommendation model is constructed based on social media sentiment, which provides investors with recommendations for next actions based on historical prices, such as 'buy', 'sell' and 'wait' 

## Introduction

互联网接入的普及引发了与流行货币体系中使用的货币不同的货币出现，基于一种称为“挖矿” 的独特方法的加密货币的出现给用户的在线经济活动带来了重大的变化。[^1] 加密货币是一种依赖于密码学的电子现金形式，它的功能与传统标准货币类似，用户能够用虚拟货币进行商品交换和服务。[^2] 加密货币与以前的货币的主要区别在于，加密货币基于去中心化控制系统，which is a digitally implemented distributed ledger, 而不是中心化银行系统。实现这种去中心化的技术是一种整体技术，其中多台计算机使用哈希算法在点对点分布式网络上同时记录和验证货币发行和交易历史。[^3] 近年来，由于传统法定货币收到安全问题以及严格的货币政策和银行监管的影响，加密货币已成为一种流行的投资选择。[^4] 自2008年比特币问世以来，到如今已经出现了数千种加密货币，它们的颠覆性潜力导致了2017年和2018年初人们对加密货币的兴趣和开发的爆炸性增长。比特币和其他加密货币价格的急剧增长也引起了人们的高度关注。

> The spread of Internet access has triggered the emergence of currencies that are different from those used in popular monetary systems, and the emergence of cryptocurrencies based on a unique method called "mining" has brought about significant changes in users' online economic activities. [^1] Cryptocurrency is a form of electronic cash that relies on cryptography and functions similarly to traditional standard currency, allowing users to exchange goods and services with virtual money. [^2] The main difference between cryptocurrencies and previous currencies is that cryptocurrencies are based on a decentralized control system, which is a digitally implemented distributed ledger, rather than a centralized banking system. The technology that implements this decentralization is a holistic technology in which multiple computers use hashing algorithms to simultaneously record and verify currency issuance and transaction histories on a peer-to-peer distributed network. [^3] In recent years, cryptocurrencies have become a popular investment option as traditional fiat currencies have received security concerns as well as strict monetary policies and banking regulations. [^4] Thousands of cryptocurrencies have emerged by now since the introduction of Bitcoin in 2008, and their disruptive potential has led to an explosion of interest and development in cryptocurrencies in 2017 and early 2018. The dramatic increase in the price of Bitcoin and other cryptocurrencies has also generated a great deal of interest.

由于加密货币市场的表现与传统货币不同，其特点是高波动性、无休市交易期、资本规模相对较小、市场数据可用性高，因此价格极难预测。[^5] 对于加密货币价格预测问题以及影响加密货币价格变动的因素也成为了近几年逐渐关注的课题，学术研究人员和专业人士都对了解这些新兴资产的行为表现出了浓厚的兴趣。根据之前的研究，比特币具有传统货币交易模式所没有的独特特征，这是因为它的价格波动取决于人们的感知和意见，而不是制度规定。[^6] 行为经济学家明确说明，金融体系的决策受到情感道德的影响，而不仅仅是资本价值的影响。[^6] Dolan和Edlin也指出，决策会受到情绪的影响。[^8] 因此，情绪分析有助于表明商品价格受到情绪和经济基本面等价值观的影响。[^9] 

> Due to the fact that cryptocurrency markets behave differently from traditional currencies, characterized by high volatility, no off-market trading periods, relatively small capitalization, and high availability of market data, prices are extremely difficult to predict. [^5] The issue of cryptocurrency price prediction and the factors affecting cryptocurrency price movements have also become a topic of growing interest in recent years, with academic researchers and professionals showing a keen interest in understanding the behavior of these emerging assets. According to previous research, Bitcoin has unique characteristics not found in traditional cryptocurrency trading models, due to the fact that its price fluctuations depend on people's perceptions and opinions, rather than institutional regulations. [^6] Behavioral economists clearly show that decisions in the financial system are influenced by emotional morality and not just by the value of capital. [^6] Dolan and Edlin also state that decisions are influenced by emotions. [^8] Thus, sentiment analysis helps to show that commodity prices are influenced by values such as sentiment and economic fundamentals. [^9]

社交媒体上的新闻和帖子强烈加剧了加密货币市场的波动。由于投资者很难判断所发布的消息是真是假，这种效应得到了进一步的加强，其中，以推特为首的社交媒体成为了加密货币投资者的主要信息来源。[^10] 对加密货币经常感兴趣的人会对某些观点发表帖子，这不仅局限于推特平台。Kristoufex是最早得出每日维基百科流量和每周谷歌搜索量价格相关的人之一。[^11] 随后，也有研究发现比特币被用户积极正面的评论显著影响着。[^ 1]  

> News and posts on social media have strongly exacerbated the volatility in the cryptocurrency market. This effect is further enhanced by the fact that it is difficult for investors to determine whether the news posted is true or false, with social media, led by Twitter, becoming the main source of information for cryptocurrency investors. [^10] People with a regular interest in cryptocurrencies will post about certain ideas, and this is not limited to the Twitter platform. kristoufex was one of the first to come up with a correlation between daily Wikipedia traffic and weekly Google search volume prices. [^11] Subsequently, studies have also found that Bitcoin is significantly influenced by positive user comments. [^ 1]

根据之前的研究，考虑到当前加密货币的投机性质和加密货币市场与社交媒体之间的联系，并且由于比特币是市场规模最大，最流行的加密货币。因此本研究致力于使用多种模型算法探索多个社交媒体平台的公众情绪与比特币价格波动之间的关系，根据公众情感分析对加密货币价格进行预测并构建投资者行为推荐模型以达到利润最大化，例如“Sell". "Buy" and "Wait". 

> Based on previous research, considering the current speculative nature of cryptocurrencies and the connection between the cryptocurrency market and social media. This study aims to explore the relationship between public sentiment on multiple social media platforms and price fluctuations of various cryptocurrencies using various modeling algorithms to predict cryptocurrency prices based on public sentiment analysis and to construct a recommendation model for investor behavior to maximize profits, such as "Sell". "Buy" and "Wait".

本研究具体要探究的问题如下：

1. 社交媒体的公众情绪与比特币价格变动之间是否真正存在关系？如果存在，公众情绪是如何影响比特币价格变动的？

   本研究将针对Twitter, Google Trends, Reddit多个平台收集比特币相关posts. 并收集同时间段内的比特币交易信息。利用统计模型探寻公众情绪与加密货币的价格变动是否存在联系。

   > Is there a relationship between public sentiment on social media and the price movements of bitcoin that actually exist? If it does, how does public sentiment affect bitcoin price movements?
   >
   > This study will extract cryptocurrency-related posts and comments online for various platforms such as Twitter, Google Trends, Reddit, etc. and collect cryptocurrency trading information online for the same time period. A statistical model will be used to investigate whether there is a connection between public sentiment and the price movement of cryptocurrencies.

2. 社交媒体公众情绪分析是否可以作为比特币价格变动和价格回报预测的有效指标？

   假设社交媒体的公众情绪对加密货币变动确实存在影响，本研究还将determine哪些社交媒体平台的公众情绪可以作为有效预测indicators for cryptocurrency price changes prediction，以及在多大程度上可以用于预测加密货币的价格变动。选用合适的特征可以最大程度地提高预测准确率。

   > Can social media public sentiment analysis be used as a valid indicator for bitcoin price movement prediction?
   >
   > Assuming that public sentiment on social media does have an impact on Bitcoin movements, this study will also determine which social media platforms' public sentiment can be used as effective predictive indicators for Bitcoin price changes prediction, and to what extent it can be used to predict cryptocurrency price changes. The selection of appropriate features can maximize the prediction accuracy.

3. 如何利用社交媒体的公众情绪构建投资者行为推荐模型，以使得利润达到最大化？

   最终，本研究intend to 构建投资者行为推荐模型，利用社交媒体情感分析提高行动推荐模型的性能，建议投资者采取相应的行动以实现利润最大化，例如"Sell", "Buy" and "Wait". 

   > Finally, this study intends to construct a recommendation model for investor behavior, use social media sentiment analysis to improve the performance of the action recommendation model, and suggest investors take appropriate actions to maximize profits, such as "Sell", "Buy" and "Wait".

针对所提出的问题，本研究的主要成就如下：

- 通过获取公开社交媒体数据集获取来源于Twitter和Reddit的公众推文，以及来源于Google Trends的比特币搜索趋势，并利用统计技术和情感分析探索了公众情绪与兴趣与比特币价格波动的相关性，及其对每小时比特币价格波动的滞后影响。且分别在短期数据和长期数据上进行了分析和测试。
- 分别对比特币的价格回报的回归预测问题及价格波动方向的分类预测问题使用多种模型进行探究，并选用最合适的预测模型。
- 利用多源的社交媒体平台数据对预测模型结果进行调整并有效提高了模型性能，选择最优价格波动分类模型构建了投资者行为推荐模型，并通过调整阈值使得价格回报最大化。

本研究的最初目标与完成情况将在以下说明：

- 收集多个社交媒体平台的比特币相关信息，利用统计技术和情感分析探索公众情绪与比特币价格波动的关系，以及公共情绪和兴趣对价格的滞后影响的时间范围。
  - Collect Bitcoin-related information from multiple social media platforms, exploring the relationship between public sentiment and Bitcoin price fluctuations and the time lagged impact using statistical techniques and sentiment analysis.
  - 由于社交媒体的访问限制，本研究最终使用了公开的历史数据集替代在线收集媒体数据。收集数据的方法及平台已在 section 2 说明，统计技术方法在 section 3进行了说明，相关性和因果测试的过程也在 scetion 4进行了实施和说明。
  
- 基于深度学习模型，选用最佳的模型对价格波动和价格回报进行预测，以获取最佳预测准确率。
  - 本研究最终选择了LSTM模型，关于模型的说明已在 scetion 5展示，其预测和评估以及优化过程在 Section 6和Section 7说明。

- 选用合适的公众情绪作为特征优化模型性能，以提高预测准确率。
  - 对不同的时期和不同价格预测任务，研究发现了不同的最佳特征组合，其结果和讨论在Section 6 和Section 7说明。
- 利用最佳预测模型构建行为推荐模型，通过调整阈值使得在特定时间范围内利润最大化。
  - 最终分别在短期数据和长期数据上构建推荐模型，其执行策略在 Section 8说明，其实施过程以及评估和对比在 Section 9说明。
- 根据时间点得出的动作推荐模型的结果是由其概率决定的。因此当预测结果为正确答案类的概率低于某个阈值时，本模型将根据社交媒体情绪分析调整预测结果。最后，本实验将使用准确率和F1 score 作为评估方法来检验模型的性能。通过增加阈值来进行相同的实验，以找到使profits最高的阈值。
- 
  - 阈值的选择也在 Section9 展示和说明。
- 将研究结果进行在线交互式可视化的构建，通过交互展现不同社交媒体公众情绪与不同加密货币之间交易信息之间的相互作用。
  - 本研究最终创建了交易模拟器，但是并没有进行构建交互式可视化的系统，第一是因为由于使用公开的媒体数据集，数据有限，无法获得不同加密货币的相关公众情绪，因此最后将焦点聚焦到了比特币的价格变动上，因此无法形成不同源的媒体数据与不同加密货币之间的关系，这也是数据的限制导致的。此外，本研究进行到后期发现公众情绪与比特币的价格变动之间的关系很难通过合适的交互式可视化进行展示，因为有些关系是隐形的，即便在图示中他们的直接相关性不强，但不意味着他们不存在因果关系。因此研究最终仍重点关注内在关系并进行验证。


In summary, 本研究将关注两种不同的数据并探索两种数据间的联系，即多源社交媒体数据和加密货币交易数据。利用社交媒体数据对加密货币价格的影响对加密货币价格变动进行预测并为投资者构建行为推荐模型以使得投资利润最大化，最后使用准确率和F1 score进行模型评估，并利用t检验进行统计验证。

The rest of this study is structured as follows: Chapter 2 will provide a general background of the cryptocurrency market and provide an extensive literature review of related studies. Chapter 3 will then discuss the methodology and subsequently, Chapter 4 will discuss the results and limitations of this work. The findings of this study are summarised in Chapter 6.

本研究的其余部分结构如下： 第 2 章将介绍加密货币市场的总体背景，并对相关研究进行广泛的文献综述。第 3 章将讨论研究方法，第 4 章将对实验进行进一步的详细阐述，并对结果进行讨论。第 5 章将对研究结果进行总结，并提出进一步的工作。





本研究收集了来源于多个社交媒体平台的公众情绪以及大众兴趣数据，其中包括来自推特的推文即每小时推文数量，Goolge Trends指数，以及Reddit的帖子，并探求了不同平台的公众情绪其对每小时比特币价格变动的影响，其中包括短期影响（仅一个月）以及长期影响（六个月）。通过统计方法对每种媒体数据与每小时比特币价格变动的相关性进行了探讨及特征选择。最终利用相关特征构建模型，对比特币价格回报和价格变动方向进行了基于短期数据和长期数据的预测，这一步也进一步验证了不同来源的公众兴趣与情绪对比特币价格回报和变动方向的滞后影响和预测能力。研究发现谷歌数据对比特币的价格回报在短期和长期趋势来看，均存在一定程度的滞后影响，但对价格方向只在长期上存在预测能力。而推文的公众情绪在短期上对价格变动方向有较强的预测能力，其可将价格变动预测准确率提高3%到8%，但在长期上推文的公众情绪影响不再显著，与谷歌趋势结合作为特征集时，可以有效提高了0.6%的平均准确率。推文数量无论在短期还是长期，与价格回报和变动都没有显著关系。而Reddit的posts显示的公众情绪仅对在长期趋势上的价格变动方向有极其微弱的影响，不能有效提高价格回报和价格变动方向的预测能力。研究仍发现，对下一小时的比特币价格进行预测时，最佳的历史时间步长在10小时到24小时，谷歌趋势一般的滞后影响在2小时或者10小时后产生，推文情绪短期内滞后影响也在10小时后产生。最终本研究利用已经使用推文公众情绪得分优化的短期最佳价格变动预测模型构建了短期交易动作推荐模型，通过调整得到最佳阈值后，相比使用原特征集的模型有效提高了最大价格回报。

总的来说，本研究的主要贡献如下：

- 探求了社交媒体数据与比特币价格变动的相关性，及不同公众情绪与兴趣对比特币价格变动的滞后影响和因果关系。
- 关注了两种比特币价格的相关预测问题：价格回报的回归预测和价格变动方向的分类预测。
- 关注并比较了不同的模型对比特币价格的预测能力，其中包括时间序列模型、机器学习模型以及深度学习模型，且发现复杂的混合模型不适用于短期预测，相反，单层的LSTM模型能产生更加的结果。
- 评估了不同源的特征在短期和长期趋势上对比特币的价格预测能力
- 利用最优模型构建了交易动作推荐模型，旨在模拟一个月的比特币买卖交易，并为下一步操作提出建议，如"Buy"或者"Sell"或"Wait"。



由于数据获取的限制，本研究只研究了来自于谷歌趋势、Twitter和Reddit的媒体数据与比特币的价格的关系。但是加密货币不止比特币一种，还有较为流行的以太币和Litecoin等。因此未来可以扩展数据的范围，不仅限于半年的数据，可以探求更长期的关系。除此以外，模型的选择由于多种，尽管通过性能评估发现表现较佳的模型仍为LSTM-based models，但是仍有其他模型可以尝试，例如利用增强学习进行研究，或者尝试使用GRU模型，以及其他混合模型。在交易动作推荐模型方面，还需要进行更多不同的数据的测试，同时，可以通过模拟器构建软件，以便用户交互和操作。

1. 我使用了Python 3作为基本编程语言，并使用了相关库如 Pandas和Numpy
2. 我使用了 Plotly 库和 Seaborn库 public-domain Python libraries 生成可视化图表
3. 我使用了Github进行版本控制和代码储存
4. 项目的早期调查工作就是以这篇论文中提供的思路为指导的：， with the code for LSTM model selection 
5. 我使用了Tensorflow, Keras库用于深度学习模型构建
6. VADER 和 TextBlob, Pysentiment2库被用于情感分析。
7. 我使用了Latex

Code was developed using a recent version of Python 3, utilising data science libraries such as NumPy and Pandas. • Version control was conducted through GitHub, ensuring that the project is securely stored and documented in the cloud. • Google Collaboratory (https://colab.research.google.com/) is used for online GPU resources and notebook-style code execution. The code is connected to cloud-hosted datasets, with all results stored in the cloud. • Tensorflow (https://www.tensorflow.org/), Keras (https://keras.io/) & PyTorch (https://pytorch.org/) are to be used for model development, training and testing. • Early investigatory work in the project was guided by the ideas provided by this notebook: https://dimartinot.com/notebooks/tiselac/, with the code for the Temporal CNN provided by https://github.com/charlotte-pel/temporalCNN from the paper by Pelletier et al. (2019) [48]. • Pre-trained networks came from the Breizhcrops library [54] with the original code hosted at: https://github.com/dl4sits/breizhcrops, with an interactive website displaying the results of each model hosted at: https://breizhcrops.org/ • I used LATEX through the online service Overleaf for the formatting of the thesis

## Literature Review

This chapter introduces necessary background and present a comprehensive review of the state-of-art study related to cryptocurrency price analysis and prediction. Particular attention is paid to past uses, documented within the literature, of social media to predict cryptocurrency prices.

### Cryptocurrency market

在2008年，新的去中心化加密货币现金系统被发布。同时，它以一种名为比特币的加密货币的形式推出了区块链技术最常见的应用[^17] . 在比特币诞生后的几年里，许多其他的加密货币被开发出来，例如Litcoin[^18]  和 Ethereum[^19].  通常情况下， 这些山寨币是为了不同的目的而开发的，或者试图改善比特币的局限性，例如比特币的供应有限，网络的高能耗等[^10]. 加密货币的相关文献最初主要是对比特币的安全、道德和法律方面的研究，然而在2017年，人们对加密货币市场的态度发生急剧变化，有起初的质疑转为爆发性的兴趣。由此引发了极端牛市，上市的加密货币数量增加了两倍多，达到了1865种。仅用一年时间，加密货币总市值从2017年初的170亿美元增长到了8130亿美元.[^10] 

> In 2008, the new decentralized cryptocurrency cash system was released. At the same time, it was launched in the form of a cryptocurrency called Bitcoin, the most common application of blockchain technology[^17] . In the years following the birth of Bitcoin, many other cryptocurrencies were developed, such as Litcoin[^18] and Ethereum[^19].  Often, these cottage coins were developed for different purposes or to try to improve the limitations of Bitcoin, such as the limited availability of Bitcoin, the high energy consumption of the network, etc[^10]. The cryptocurrency-related literature initially focused on the security, ethical, and legal aspects of Bitcoin, however, in 2017, attitudes towards the cryptocurrency market changed drastically, with an initial skepticism turning into an explosion of interest. This triggered an extreme bull market, with the number of listed cryptocurrencies more than tripling to 1,865. In just one year, the total market capitalization of cryptocurrencies grew from \$17 billion at the beginning of 2017 to \$813 billion . [^10]

有关加密货币是什么类型的资产类别的争议也一直存在。Selgin认为，尽管人们声称比特币应被视为投机商品而不是货币，投资者仍将比特币用作货币并用于投资目的[^21]. Cheah 和 Fry认为，如果比特币是一个真正的单位或账户，或者是一种价值储存形式，它就不会表现出泡沫和崩盘所表现出的波动性[^22] . Yenmack 将比特币的稀缺性和不稳定性视为其不被归类为真正的货币的的原因。[^23] 美国证券交易委员会2018年报告称，比特币和以太坊不能被归类为证券，但可能更类似于商品。[^24] 此外，Dwyer发现比特币的平均月波动率高于黄金或一组外币，并且比特币的最低月波动率小于黄金和货币的最高月波动率。[^22] Dyhrberg发现比特币具有与黄金和美元类似的对冲能力，因此可以利用风险管理。[^25] 

> Controversy also persists regarding what type of asset class cryptocurrencies are. selgin argues that investors use bitcoin as a currency and for investment purposes despite claims that it should be considered a speculative commodity rather than a currency[^21] . Cheah and Fry argue that if bitcoin were a real unit or account, or a form of store of value, it would not exhibit the volatility exhibited by bubbles and crashes[^22] . Yenmack sees the scarcity and instability of bitcoin as the reason it is not classified as a real currency. [^23] The SEC reported in 2018 that bitcoin and ethereum cannot be classified as securities, but may be more akin to commodities. [^24] In addition, Dwyer found that Bitcoin's average monthly volatility is higher than that of gold or a group of foreign currencies, and that Bitcoin's minimum monthly volatility is less than the maximum monthly volatility of gold and currencies. [^22] Dyhrberg found that bitcoin has similar hedging capabilities to gold and the U.S. dollar and therefore can be utilized for risk management. [^25]

在加密货币中，每种货币都有自己的用途，因此很难知道针对比特币的研究结果是否可以推广到所有加密货币中，也很难定义加密货币属于哪种资产类别，因为它们具有各种现有资产类别的特征。甚至可以说，加密货币形成了一种全新的类别[^10] .

近年来，随着比特币价格连续下跌，导致整个加密货币市场受到拖累，市场参与者对造成这种低迷的因素产生了浓厚兴趣。加密货币交易者认为，只要价格走势可预测，涨跌都能够获利。在预期繁荣期，投资者可以提前买入加密货币，等待价格上涨后获得回报。而在未来可预见的衰退期，投资者可以通过保证金交易的方式卖空加密货币，从中获取超额收益。此外，自从CBOE在2017年推出比特币期货后，持有多头或空头头寸变得更加容易。这种金融工具使得投资者甚至无需实际持有比特币，就可以利用杠杆进行双向投机。最近，通过在离岸交易所进行二元期权交易，也可以对其他加密货币采取类似的策略。[^39] 

> In cryptocurrencies, each has its own use, so it is difficult to know whether the findings for Bitcoin can be generalized to all cryptocurrencies and to define which asset class cryptocurrencies belong to, as they have characteristics of various existing asset classes. One could even argue that cryptocurrencies form an entirely new category[^10] .
>
> In recent years, as the continuous decline in the price of Bitcoin has led to a drag on the entire cryptocurrency market, market participants have taken a keen interest in the factors that have contributed to this downturn. Cryptocurrency traders believe that they can profit from both ups and downs as long as price movements are predictable. During an expected boom period, investors can buy cryptocurrencies in advance and wait for the price to rise to reap the rewards. And during foreseeable future downturns, investors can sell cryptocurrencies short by trading on margin and reap the excess returns. Furthermore, since the CBOE launched bitcoin futures in 2017, it has become much easier to hold long or short positions. This financial instrument allows investors to use leverage to speculate in both directions without even physically holding bitcoin. More recently, a similar strategy has been possible with other cryptocurrencies by trading binary options on offshore exchanges. [^39]

研究者们对加密货币价格是否可预测提出了疑问，即有效市场假说（EMH）是否适用于加密货币市场。在有效市场中，所有过去的信息应该已经反映在当前价格中，而价格可能只受未来事件的影响[^40] . 然而，由于未来是不确定的，价格应该遵循随机游走[^39] 。在弱式效率的情况下，过去的价格变化不能预测未来的回报[^41] 。近年来，对比特币的定价效率进行了广泛研究，其中Urquhart提供了早期证据，认为比特币市场并非弱效率，但随着时间推移，效率有可能变得较弱[^42] 。

> Researchers have questioned whether cryptocurrency prices are predictable, i.e., whether the efficient market hypothesis (EMH) is applicable to the cryptocurrency market. In an efficient market, all past information should already be reflected in the current price, which may only be influenced by future events[^40] . However, since the future is uncertain, prices should follow a random wander[^39] . In the case of weak-style efficiency, past price changes do not predict future returns[^41] . The pricing efficiency of Bitcoin has been extensively studied in recent years, with Urquhart providing early evidence that the Bitcoin market is not weakly efficient, but that it is likely to become weaker over time[^42] .

在比特币市场中，价格表现出复杂的、混乱的动态，并且在高价时期，回报的不确定性显著增加。这种复杂性是由市场中多种相互作用的因素和不确定性引起的，包括经济和政治条件以及人类行为。尽管一致地预测市场价格走势相当困难，但并非不可能[^5]。以前的研究也表明，在金融预测中，盈利并不一定要准确预测未来价格的具体数值。实际上，相对于价格的确切价值，预测市场方向可以带来更高的利润。[^43] 

> In the Bitcoin market, prices exhibit complex, chaotic dynamics and, in periods of high prices, significantly increased uncertainty in returns. This complexity is caused by multiple interacting factors and uncertainties in the market, including economic and political conditions as well as human behavior. Although it is quite difficult to predict market price movements consistently, it is not impossible [^5]. Previous studies have also shown that in financial forecasting, earnings do not necessarily have to accurately predict the exact value of future prices. In fact, predicting the market direction can lead to higher profits relative to the exact value of the price. [^43]



### Cryptocurrency Price Prediction



本小节简单回顾了过去使用机器学习和深度学习方法对比特币价格进行预测的研究。几十年来，利用日常数据和可访问的高频数据进行股市预测已经不断发展[^98]。然而，如何预测比特币价格的研究仍然缺乏。之前的研究通过两种方式预测比特币价格：实证分析和稳健的机器学习算法分析。机器学习算法已经广泛应用到许多领域进行准确预测，简单的复制机器学习算法会因方法复杂而导致过拟合等问题，因此，Zheshi等人关注了将不同建模技术应用于具有不同数据结构和维度特征的样本的可行性。他们按每日价格和高频价格对比特币进行分类，采用了资产与网络，行情，黄金现货价格等一系列高维特征，并使用随机森林、XGBoost以及支持向量机等机器学习方法，其准确率可达66% [^98]。

> This subsection provides a brief overview of prior research pertaining to Bitcoin price prediction using machine learning and deep learning methodologies. The domain of stock market forecasting using daily and readily accessible high-frequency data has evolved over several decades[^98]. Nevertheless, a dearth of comprehensive research focusing on Bitcoin price prediction remains. Previous studies have approached Bitcoin price prediction through empirical analysis and robust machine-learning algorithm evaluation.
>
> Machine learning algorithms have gained prominence for their capacity to yield precise predictions across diverse domains. Yet, the direct replication of these algorithms can introduce challenges, including overfitting, due to the intricate nature of the methodology. Zheshi et al. sought to address this by concentrating on the applicability of distinct modelling techniques to datasets characterised by varying structural and dimensional attributes. Through the classification of Bitcoin data based on daily and high-frequency prices, the study harnessed an array of high-dimensional features encompassing assets and networks, quotations, and the spot price of gold. Employing machine learning methodologies such as Random Forests, XGBoost, and Support Vector Machines, the research achieved a notable accuracy of up to 66%[^98]."

加密货币价格数据也是一种时间序列数据，因此有大量使用ARIMA等时间序列预测模型对比特币价格进行预测的研究。2019年，Amin通过分析 3 年时间段内的价格时间序列，揭示传统自回归综合移动平均线 (ARIMA) 模型在预测比特币未来价值方面的有用性 [^101]，实证研究表明，这种简单的方案在时间序列行为几乎不变的子周期中是有效的，特别是当它用于短期预测时。当研究人员将 ARIMA 模型训练到 3 年之久，在此期间比特币价格经历了不同的行为，再次尝试使用它进行长期预测时，此模型会引入较大的预测误差。特别是，ARIMA模型无法捕捉价格的剧烈波动。同时，研究表明，时间窗口的位置及其长度对价格预测的结果也会产生影响 [^101]。Triyanna等人也利用ARIMA模型对2013年到2019年的比特币数据进行以day为单位的短期预测研究，结果发现ARIMA可以提前1到7天进行比特币价格预测，并能取得较好的结果 [^74]。

> Cryptocurrency price data, categorized as a type of time-series data, has prompted substantial research employing time-series forecasting models, including ARIMA, to predict Bitcoin's price. In 2019, Amin demonstrated the utility of the conventional Autoregressive Integrated Moving Average (ARIMA) model in projecting Bitcoin's future value. This was achieved through an analysis of price time series spanning a 3-year timeframe[^101]. Empirical investigations have indicated the efficacy of this straightforward approach, particularly within subperiods characterized by relatively stable time series behaviour, especially in the context of short-term forecasting.
>
> However, issues arise when employing the ARIMA model for long-term forecasting, following training over a 3-year span. Given Bitcoin's varied price behaviour over that period, the model tends to yield significant forecasting errors, particularly when faced with abrupt price fluctuations. Moreover, the research underscores that the positioning and duration of the time window exert an influence on price prediction outcomes[^101].
>
> Triyanna et al. similarly leveraged the ARIMA model for short-term prediction within a day-based context, spanning Bitcoin data from 2013 to 2019. Their study revealed ARIMA's capability to forecast Bitcoin prices 1 to 7 days ahead, yielding favourable results[^74].

Sean等人利用贝叶斯优化RNN和LSTM网络对价格移动方向进行了预测，并将其与常用的时间序列预测模型ARIMA进行对比，其揭示了深度学习网络表现确实优于传统的时间序列模型ARIMA[^99]。2019年，Suhwan等人研究并比较了各种先进的深度学习方法，例如DNN、LSTM模型、卷积神经网络、深度残差网络、以及它们的组合用于比特币价格预测[^100]。实验结果表明，虽然基于 LSTM 的预测模型在比特币价格回归预测方面略优于其他预测模型，但基于 DNN 的模型在价格涨跌预测方面表现最好。同时，研究人员提出，回归的最佳预测模型不一定是分类的最佳模型，反之亦然[^100]。

> Sean et al. employed Bayesian-optimized RNN and LSTM networks to predict price movement direction and contrasted their performance with the conventional time series prediction model, ARIMA. The study underscored that deep learning networks exhibit superior performance over the traditional ARIMA model[^99]. Similarly, in 2019, Suhwan et al. conducted a comprehensive investigation and comparison of cutting-edge deep learning methodologies. This encompassed DNNs, LSTM models, convolutional neural networks, and deep residual networks, as well as their amalgamations, for predicting Bitcoin price[^100]. Empirical findings indicated that while LSTM-based prediction models showcased a slight advantage in Bitcoin price regression forecasting, the DNN-based model excelled in predicting price rises and falls. Notably, the research emphasised that the most effective prediction model for regression might not necessarily be the optimal choice for classification, and vice versa[^100].

Mohammed等人在2021年提出了一种基于机器学习的，使用高纬度特征对比特币价格进行时间序列预测的方法 [^76]。该研究使用了ANN、SVM等机器学习模型来预测短期到中期的比特币价格，该预测考虑了将所有的价格指标作为特征，包括 block size和hash rate等。在所有模型中，结果下那是LSTM在每日价格分类预测方面表现出了最好的性能，对次日的预测可达65% [^76]。

> In 2021, Mohammed and his team suggested using high-latitude features for a machine learning-based time-series prediction of Bitcoin prices. Their study employed various machine learning models like SVM, ANN, and others to forecast the price in the short to medium term. The study included all the price metrics as features, including block size and hash rate. The LSTM model performed the best among all the models and showed up to 65% prediction accuracy for the next day's daily price classification.

除此之外，也有少量使用混合模型及高维特征对比特币价格预测的研究，Saúl等人提出了基于CNN和LSTM的CNN-LSTM混合模型 [^86]，将其与高频市场的技术指标结合对加密货币价格日内趋势进行分类预测，并将其结果与CNN和LSTM以及多层感知器进行比较，实验结果表示，提出的混合模型的结果总是优于其他网络，尽管对于大多数加密货币，CNN和LSTM已经适合预测价格移动方向。同时，混合模型是是唯一能够预测 Dash 和 Ripple 价格走势的网络 [^86]。此外，Yan等人也提出了一种CNN-LSTM混合模型结构对比特币价格进行回归和分类预测，该方法考虑了将宏观经济变量、投资者关注度等外部信息作为输入 [^103]，并使用了跨越两年时间的数据集。最终同样得到了CNN-LSTM混合模型在比特币价格预测中表现较为优秀的结论。

> Furthermore, several studies have explored Bitcoin price prediction using hybrid models and high-dimensional features. Saúl et al. introduced a CNN-LSTM hybrid model that combines CNNs and LSTMs to predict intraday trends in cryptocurrency prices. This hybrid model incorporates technical indicators from the high-frequency market and was compared with standalone CNNs, LSTMs, and multilayer perceptron networks. The experimental results consistently demonstrated the superiority of the proposed hybrid model, particularly in predicting Dash and Ripple prices[^86].
>
> Similarly, Yan et al. proposed a CNN-LSTM hybrid model for both regression and classification predictions of Bitcoin prices. This model incorporated external variables like macroeconomic indicators and investor sentiment[^103]. Utilizing a two-year dataset, the study found that the CNN-LSTM hybrid model consistently outperformed other approaches in Bitcoin price prediction. This further underscores the potential benefits of hybrid models and the incorporation of diverse data sources for enhancing cryptocurrency price predictions.

在过往研究调研中，可以发现，针对比特币价格预测的研究大多偏向于以day为单位，且数据大多跨越几年。性能优秀的模型经常与市场交易信息结合，这要求有更多的技术指标输入。本研究将不关注于技术指标特征，而将重点转移到社交媒体信息对比特币价格的影响和预测能力。

> Previous research studies have shown that most Bitcoin price prediction studies are biased towards certain days and often span several years. Successful models often incorporate market trading information and require in-depth technical indicator inputs. However, this study aims to shift the focus towards the impact and predictive power of social media information on Bitcoin prices, rather than relying on technical indicators.

### Sentiment analysis

传统金融市场已经在上个世纪建立起来，拥有大量可供研究或商业使用的结构化数据[^26] . 对于加密货币市场来说，结构化数据并不那么容易获得，对替代数据源的需求在预测回报方面发挥着关键作用。[^27] 在经济学背景下，Kaplanski et.al 将情绪定义为任何可能导致资产基本价值错误定价的误解。因此情绪会使资产具有投机性。[^33] 

用于加密货币回报预测的文本数据自然语言处理方法包括主题建模[^28]，语义分析[^29] ，和情感分析[^30] 。本研究重点关注情感分析，其是自然语言处理的一个子领域，是人们对实体、个人、问题、事件、主题及其属性的意见、评价、态度和情感的计算研究[^34] ，其主要目标是识别一段文本的polarity,[^31] 分配一个非结构化文本的积极、消极或中性情绪极性得分。情感分析包含许多库，例如"NLTK", "VADER","TextBlob" and "Flair" 都提供基于深度学习的情感分析模型[^13] . 情感分析方法已经从基于词典的方法发展到了最先进的Transformer模型[^32]. 

> Natural language processing methods for text data for cryptocurrency return prediction include topic modeling [^28], semantic analysis [^29], and sentiment analysis [^30]. This study focuses on sentiment analysis, a subfield of natural language processing, which is the computational study of people's opinions, evaluations, attitudes, and sentiments about entities, individuals, issues, events, topics, and their attributes [^34], with the main goal of identifying the polarity of a text, [^31] assigning a positive, negative, or neutral emotional polarity score to an unstructured text. Sentiment analysis includes many libraries, such as "NLTK", "VADER", "TextBlob" and "Flair" that provide deep learning-based sentiment analysis models[^13] . Sentiment analysis methods have evolved from lexicon-based approaches to the state-of-the-art Transformer model [^32].

由于社交媒体平台的广泛使用，情感分析已成为一个重要的研究领域。Hasan et.al 提出了一种机器学习方法，用于从社交媒体上发布的文本中通过执行文本分类来自动检测情绪。该研究探讨了文本消息语义复杂性、文本中的多重情感以及不同情绪状态等问题。二元分类用于区分带有情感的推文和不带情感的推文。该方法的两个主要任务包括离线训练和在线分类任务。开发的情感分类系统对于文本消息可以获得90%的分类准确率。[^35] 

> Sentiment analysis has become an important area of research due to the widespread use of social media platforms. hasan et.al proposed a machine learning approach for automatic detection of sentiment from texts posted on social media by performing text classification. The study explores issues such as semantic complexity of text messages, multiple sentiments in texts, and different emotional states. Binary classification is used to distinguish between tweets with sentiment and tweets without sentiment. The two main tasks of the approach include offline training and online classification tasks. The developed sentiment classification system can obtain 90% classification accuracy for text messages. [^35]

Sasmaz et.al 专注于NEO加密货币的情绪分析以及推文的每日情绪和NEO市场价格的相关性。[^36] 通过使用随机森林和BERT分类起构建模型来进行情感分析实验，研究发现前一种算法优于另一种算法。在该研究的第二阶段，将推文的每日情绪与NEO的市场价格进行了比较。

> Sasmaz et.al focused on sentiment analysis of NEO cryptocurrencies and the correlation between daily sentiment of tweets and NEO market prices. [^36] By using Random Forest and BERT classification to build models for sentiment analysis experiments, the study found that the former algorithm outperformed the other one. In the second phase of the study, the daily sentiment of tweets was compared with the market price of NEO.

此外，Naila等人使用与加密货币相关的推文进行情绪分析和情绪检测，为提高分析的效率，该研究提出了一种深度学习集成模型LSTM-GRU。LSTM-GRU模型结合了两种循环神经网络应用，包括长短期记忆模型(LSTM)和门控循环单元(GRU)，并将两者堆叠在一起，最终利用GRU根据LSTM提取的特征进行训练。研究发现使用Bow特征时模型的性能相对较好，LSTM-GRU集成模型在情感分析方面显示出0.99的准确率，在情感预测方面显示出0.92的准确率。[^37] 

> In addition, Naila et al. used cryptocurrency-related tweets for sentiment analysis and sentiment detection. to improve the efficiency of the analysis, the study proposed a deep learning integration model LSTM-GRU. the LSTM-GRU model combines two recurrent neural network applications, including the long short-term memory model (LSTM) and the gated recurrent unit (GRU), and stacks the two together. The GRU was eventually used to train based on the features extracted from the LSTM. It was found that the performance of the model was relatively good when Bow features were used, and the integrated LSTM-GRU model showed an accuracy of 0.99 for sentiment analysis and 0.92 for sentiment prediction. [^37]

最近，Gül et.al使用基于积极和消极情绪的推文进行与加密货币相关的文本情绪分类，并引入了一种新颖的深度神经网络混合架构以提高情感分析的准确率，该架构使用卷积层导出局部特征，并使用分组增强机制开发与直观特征相关的权重值。[^38] 将改进的上下文向量输入双向层以获取全局特征后，采用了注意机制和全连接层。实验结果表明所提出的架构准确率为93.77%，优于最先进的架构。[^38] 

> Recently, Gül et.al used tweets based on positive and negative sentiments for text sentiment classification related to cryptocurrencies and introduced a novel deep neural network hybrid architecture to improve the accuracy of sentiment analysis, which uses a convolutional layer to derive local features and a grouping enhancement mechanism to develop weight values associated with intuitive features. [^38] An attention mechanism and a fully connected layer are employed after feeding improved context vectors into a bidirectional layer to obtain global features. Experimental results show the accuracy of the proposed architecture is 93.77%, which is better than the state-of-the-art architecture. [^38]

### Sentiment analysis-based cryptocurrency prediction

鉴于大量的经验证据表明，从投资相关的社交媒体和其他在线平台中提取的情绪分数反映了投资者的主观看法，许多研究调查了在（加密货币）回报预测模型中添加相应特征的潜在价值。[^27] 在2015年， Ifigeneia等人使用时间序列分析来研究比特币价格与基本经济变量、技术因素以及来自Twitter信息的集体情绪测量之间的关系。使用机器学习算法——支持向量机(SVM),进行情感分析。短期分析表明Twitter的情绪比率、维基百科搜索查询数量以及哈希率对比特币的价格有积极影响。相反，比特币的价值受到美元和欧元汇率的负面影响[^44] 。2016年，Young等人对在线社区中的用户评论进行分析来预测加密货币的价格和交易数量。结果表明，网络社区中的用户评论和回复被证明会影响用户之间的交易数量。[^1] 随后，他们又在此基础上进行拓展，分析了比特币用户经常提到的主题，并将其含义与比特币交易联系起来。[^45] 

> Given the large body of empirical evidence that sentiment scores extracted from investment-related social media and other online platforms reflect investors' subjective perceptions, many studies have investigated the potential value of adding corresponding features to (cryptocurrency) return prediction models. [^27] In 2015, Ifigeneia et al. used time series analysis to investigate the relationship between bitcoin prices and underlying economic variables, technical factors, and collective sentiment measures from Twitter messages. Sentiment analysis was performed using a machine learning algorithm, support vector machine (SVM). The short-term analysis shows that the sentiment ratio of Twitter, the number of Wikipedia search queries, and the hash rate have a positive impact on the price of Bitcoin. In contrast, the value of Bitcoin is negatively affected by the exchange rate of the US dollar and the euro[^44] .In 2016, Young et al. analyzed user comments in online communities to predict the price and number of transactions of cryptocurrencies. The results showed that user comments and replies in online communities were shown to affect the number of transactions between users. [^1] They then expanded on this by analyzing topics frequently mentioned by Bitcoin users and linking their meaning to Bitcoin transactions. [^45]

Galen发现Twitter情绪与人们的总体情绪状态相关，此外，他发现像Twitter这样的社交媒体往往会对最终用户产生镇静作用，而不是放大他们的情绪状态[^9] . 基于此发现，2018年，Jethin等人提出了一种利用Twitter数据和谷歌趋势数据预测比特币和以太坊价格变化的方法，他们使用VADER对文本数据进行情感分析，并将结果输入到线性模型中进行预测。研究发现谷歌趋势数据和推文量更好地反映了人们对拥有加密货币的整体兴趣。[^47] Krzysztof在此基础上进行了进一步的研究，他使用了基于多个机器学习模型的混合模型对加密货币价格进行预测，以缓解任何一种模型的一些缺陷，结果发现谷歌趋势数据和一般负面情绪的结合是最有力的预测因素。[^6] Smuts使用Google Trends和Telegram情绪，通过LSTM模型进行了基于二元情绪的价格预测方法[^50] 。2018年上半年测试集上的回测预测每小时价格的准确率达到了76%。[^51] 

> Galen found that Twitter sentiment correlated with people's overall emotional state, and furthermore, he found that social media like Twitter tended to have a calming effect on end users rather than amplifying their emotional state[^9] . Based on this finding, in 2018, Jethin et al. proposed a method to predict bitcoin and ethereum price changes using Twitter data and Google Trends data, where they used VADER to perform sentiment analysis on text data and input the results into a linear model for prediction. The study found that Google Trends data and the volume of tweets better reflected the overall interest in owning cryptocurrencies. [^47] Krzysztof built on this by using a hybrid model based on multiple machine learning models to predict cryptocurrency prices to mitigate some of the shortcomings of any one model, and found that a combination of Google Trends data and general negative sentiment was the strongest predictor. [^6] Smuts performed a binary sentiment-based price prediction method using Google Trends and Telegram sentiment via an LSTM model [^50] . backtest predictions on the first half of 2018 test set achieved 76% accuracy for hourly prices. [^51]

Tiran等人采用大数据方法，使用来自Twitter、Telegram和Reddit的数百万个帖子样本来研究社交媒体平台如何以及是否影响比特币的价格和交易量，以及其他15个顶级货币的价格和交易量。研究结果证明，交易量可以预测比特币价格的波动，但主要是预测交易量，且Reddit和Telegram的帖子对比特币交易量的影响比Twitter更大。[^22] 此外，Kwansoo等人使用Hidden Markov Model(HMM) 并基于六个月的Twitter、Google文本数据研究了加密货币市场由于牛市或熊市期间与社会情绪的相互作用而发生变化的程度。研究结果表明，与熊市相比，牛市期间的社会情绪相对相关；处于局部下降趋势的加密货币市场往往更容易对积极的社会情绪做出反应；处于局部上涨趋势的市场往往与社会负面情绪更好地互动。[^49] 

> Tiran et al. used a big data approach using a sample of millions of posts from Twitter, Telegram, and Reddit to examine how and if social media platforms affect the price and trading volume of Bitcoin, as well as the price and trading volume of the other top 15 currencies. The results of the study demonstrate that trading volume predicts fluctuations in the price of Bitcoin, but mainly the volume of transactions, and that Reddit and Telegram posts have a greater impact on Bitcoin trading volume than Twitter. [^22] Furthermore, Kwansoo et al. used Hidden Markov Model (HMM) and based on six months of Twitter, Google text data to study the extent to which the cryptocurrency market changes due to the interaction with social sentiment during a bull or bear market. The findings show that social sentiment is relatively correlated during bull markets compared to bear markets; cryptocurrency markets in localized downtrends tend to be more responsive to positive social sentiment; and markets in localized uptrends tend to interact better with negative social sentiment. [^49]

Poongodi等人使用主题建模的方法探究比特币论坛talks与价格波动之间的关系，他们构建了一个文档术语矩阵，包含每个文档的单词频率，并应用LDA模型了解那些主题在特定的时间段最受欢迎，该模型有效证明了使用社交媒体数据预测全球加密货币价格的前景十分乐观。[^52]  RAJ等人提出了一种用于加密货币价格预测的混合框架DL-Gues，该框架考虑了其与其他加密货币以及市场情绪的相互依赖性，结合LSTM与GRU模型，并使用加密货币的历史价格和最近的Twitter情绪来预测货币价格，其MSE低至0.0185。[^53] Zi等人也利用LSTM与GRU两种模型，使用堆叠集成技术来提高决策的准确性。除了社交媒体评论外，该模型也选择了技术指标作为输入，其实验结果证明近实时预测具有更好的性能，其平均误差(MAE) 比每日预测好88.74%。[^54] 

> Poongodi et al. used a topic modeling approach to explore the relationship between bitcoin forumtalks and price fluctuations, they constructed a document term matrix containing the word frequencies of each document and applied an LDA model to understand those topics that are most popular at a specific time period, the model effectively demonstrated that the use of social media data to predict global cryptocurrency prices is promising. [^52] RAJ et al. proposed a hybrid framework for cryptocurrency price prediction, DL-Gues, which considers its interdependence with other cryptocurrencies and market sentiment, combines LSTM and GRU models, and uses historical prices of cryptocurrencies and recent Twitter sentiment to predict currency prices with MSE as low as 0.0185.[^53] Zi et al. also utilized both LSTM and GRU models and used stacking integration techniques to improve the accuracy of their decisions. In addition to social media comments, the model also chose technical indicators as input and their experimental results proved that near real-time prediction has better performance with a mean error (MAE) 88.74% better than daily prediction. [^54]

在前人的基础上，Man-Fai等人提出了一种智能系统，利用社交媒体情绪分析、价格跟踪系统和机器学习技术来生成加密货币交易信号。该系统包括一个用于显示加密货币价格数据的实时价格可视化组件和一个预测功能，根据前一天的加密货币推文的情绪评分提供短期和长期交易信号。此外，他们在社交媒体情感分析部分做出了改进，使用CNN-LSTM模型代替TextBlob模型以提高情感分类的准确率，之后使用随机森林回归模型代替决策树模型对加密货币价格进行预测。[^55]  Duygu等人发现使用若标签进行微调可以增强基于文本的特征的预测价值，并提高预测加密货币回报的预测准确性。[^27] 

> Building on previous work, Man-Fai et al. propose an intelligent system that uses social media sentiment analysis, a price tracking system, and machine learning techniques to generate cryptocurrency trading signals. The system includes a real-time price visualization component for displaying cryptocurrency price data and a prediction function that provides short- and long-term trading signals based on the sentiment scores of the previous day's cryptocurrency tweets. In addition, they made improvements in the social media sentiment analysis component by using a CNN-LSTM model instead of a TextBlob model to improve the accuracy of sentiment classification, followed by a random forest regression model instead of a decision tree model for cryptocurrency price prediction. [^55] Duygu et al. found that fine-tuning using wake-tags enhanced the predictive value of text-based features and improved the predictive accuracy of predicting cryptocurrency returns. [^27]

基于以上研究可以发现，如今大部分的研究都集中于使用Twitter公众情绪来对比特币价格进行回归预测，基于多社交平台对比特币影响的短期预测的研究较少。此外，之前的研究表明，关注价格走向比预测具体的价格更有意义，但是许多使用复杂深度学习的模型都关注于具体数值预测而非预测价格走向。因此本研究将在以上研究的基础上进行改进，关注多个社交平台在短期内对比特币价格方向的影响及预测效果，以探寻预测加密货币价格走向的最优指标和模型。

> According to the above studies, most of the studies today focus on using Twitter public sentiment to make regression predictions on Bitcoin prices, and there are fewer studies based on multiple social platforms and multiple cryptocurrencies. Also most studies do not make improvements in sentiment analysis and basically use VADER or TextBlob methods. In addition, previous studies have shown that it is more meaningful to focus on price direction than to predict specific prices, but many models using complex deep learning focus on specific value prediction rather than predicting price direction. Therefore, this study will improve on the above research by focusing on multiple social platforms and multiple cryptocurrencies to explore the optimal metrics and models for predicting the price direction of cryptocurrencies.



### Google Trends Analytics

本研究考虑的另一个因素Google Trends的数据，这是 Google 于 2006 年 5 月发布的应用程序，它显示了给定搜索词进入 Google 搜索引擎的频率[^94]。

> This study incorporates data from Google Trends, a platform introduced by Google in May 2006, which showcases the frequency of specific search terms being queried within the Google search engine[^94]. Google Trends delivers information in the form of relative search volume scores, representing the popularity of a given search term over a designated time span[^6]. Researchers have harnessed this data to prognosticate stock market trends[^92]. Melody et al., within a Granger causality framework, reaffirmed the legitimacy of Google Trends as a reliable metric for gauging investor attention, thereby influencing the anticipation of stock market shifts [^95]. In 2013, Kristoufek's predictive model underscored Google Trends as a significant factor influencing Bitcoin prices, evidenced by its notably low p-value [^93]. The study illuminated that heightened interest corresponded to price surges; conversely, during instances when prices trended lower, intensified interest further contributed to downward pressure.In 2018, Abraham et al. harnessed Google Trends data to engage in linear modeling, thus accurately predicting the direction of cryptocurrency price changes [^47]. Subsequently, in 2019, Nico's investigation into Google Trends' search analytics behavior unearthed nuanced dynamics within cryptocurrency price behavior. The findings revealed that the connection between cryptocurrency price movements, as identified by Google Trends, and internet search volumes was not universally positively correlated. Specifically, a robust negative correlation emerged between Bitcoin and Ether during June 2018 [^50]. This analysis, conducted using an LSTM model, further established Google Trends as a potent predictor of Ether price movements [^50]. In 2021, Maximilian et al. meticulously analyzed a gamut of metrics to forecast hourly fluctuations in Bitcoin prices, incorporating Google Trends data. Employing models such as VAR, SARIMAX, LSTM, and BiLSTM, the study highlighted a moderate correlation between Bitcoin prices and Google Trends data scales [^97].
>
> The research found that there are fewer studies on the correlation of cryptocurrency hourly changes with Google Trends data and most studies focus on the accuracy of predictions with daily prices. However, since cryptocurrencies are highly oscillating markets, further validation and research are necessary to determine the relationship between Google Trends data and Bitcoin. 
>
>  

Google 趋势提供由给定时间间隔内给定搜索词的相对搜索量得分组成的数据[^6]， 一些研究人员使用 Google 趋势数据来预测股市[^92]。Melody等人在格兰杰因果框架的背景下重新评估了谷歌趋势是衡量投资者注意力的有效指标，这对预测股票市场的移动方向产生了影响[^95]. 2013年，Kristoufek[^93]的预测模型表明，谷歌趋势是影响比特币价格的因素之一，谷歌趋势的p值较低，但结果非常显着, 他们发现虽然价格很高，但兴趣的增加将价格进一步推高。从相反的角度来看，如果价格低于趋势，不断增长的兴趣会将价格推得更深。Abraham等人在2018年应用谷歌趋势数据应用线性建模使得能够准确预测加密货币价格变化的方向[^47]。 2019年，Nico对Google Trends的搜索分析行为进行了调查，以确定它与加密货币价格行为之间的关系，结果表明，与早期的研究结果相反，从谷歌趋势获得的加密货币价格走势与互联网搜索量之间的关系不再始终呈正相关，2018 年 6 月期间比特币和以太坊检测到强烈的负相关性，此实验利用了LSTM模型，Google Trends 被证明是以太坊价格变动的的良好的预测器[^50]。2021年，Maximilian等人为对比特币价格每小时的变化进行预测，研究了多种预测比特币价格的指标，其中包含Google Trends数据。该研究使用了VAR, SARIMAX, LSTM和BiLSTM等模型进行研究，结果表明，Bitcoin价格与Google 趋势规模数据可能具有适度的相关性[^97]。调研发现，关于加密货币每小时变化与Google趋势数据相关性的研究较少，大部分研究更关注与每日价格的预测准确性，但是加密货币是高震荡的市场，因此对Google Trends数据与比特币的关系也需要进一步验证和研究。



### Action Recommendation Model

在加密货币市场中，重要的不仅是价格预测的实际价值，而且是短期内的利润最大化。[^13] 然而现有的模型存在局限性，大部分的模型只进行了具体数值的预测，投资者必须自行进行计算并判断行动以实现利润最大化。为了打破这种局限性，推荐模型会对用户进行行为推荐，例如“Sell”, “Buy” and “Wait”，来使利益最大化。然而，和价格预测模型相比，对于行为推荐的模型研究较少且模型性能较差。[^13] 

For stock price, Nelson et al. compared the performance between LSTM, Multi-layer perceptron (MLP), Random Forest (RF), and pseudo-random-based action recommendation models for two action classes (“Sell” and “Buy”) , The results showed that the LSTM-based model outperformed other models with a mean accuracy of 0.56. [^14] Sanboon et al. 也做了相似的研究，他们比较了LSTM, MLP, RF, SVM, Logistic regression, decision tree, and KNN-based action recommendation 之间的推荐性能， 根据实验结果， LSTM-based model showed the highest performance compared to the other models.[^15] 

For Cryptocurrency, Park et al. proposed three classes(“sellProfit”, “buyProfit”, and “maxProfit”), which were input features based on the predicted next-day cryptocurrency price, to improve the performance of the action recommendation model. They performed the experiment on the LSTM-based classification model using the price data of BTC, ETH, Cardano, Dash, LTC, and Monero for 1,339 days from January 1, 2018, to August 31, 2021. According to their experiment, the performance improved by approximately 13% to 21% compared to when the proposed three input features were not used, and the result was statistically validated. [^16] 

Recently, Park et al. proposed a new method for adjusting the result of the action recommendation model based on Twitter sentiment analysis, which improved the performance by approximately 3% compared to the aforementioned methods.[^13]

As can be seen from the studies above, **research in this aspect has very low performance, and there are no in-depth studies. Furthermore, cryptocurrency prices are affected by various factors, not just Twitter tweets. However, this study proposed an adjustment method that focused only on Twitter tweets. Therefore, we will conduct follow-up studies in the future to overcome the aforementioned limitations.** [^13]



## Project Timeline

- Week1: 在第一周，我将确定大概的研究方向并制定一个粗略的计划，收集相关文献和信息。(25. May - 1. June)

  > Determine the general field of my research and develop a rough plan, gather the relevant literature and information.

- Week2: 第二周，我将收集所有相关文献并按照不同类别进行整理，文献方向包括加密货币相关研究、情感分析相关研究、使用情感分析对加密货币的预测、以及关于加密货币交互式可视化的研究。除此之外，学习加密货币相关的基础概念，以便能够看懂论文。(1. June - 8. June)

  > I will collect all the related literature and sort it into different categories, in the fields of cryptocurrency-related research, sentiment analysis-based research, prediction of cryptocurrencies using sentiment analysis, and research on interactive visualisation of cryptocurrencies. In addition to this, I will learn the basic concepts related to cryptocurrencies in order to ba able to understand the concepts in the paper.

- Week3: 按照不同分类浏览和筛选论文，了解课题的已有研究并做笔记，除此之外，尽力找出现有研究的改进方向。过滤对课题意义不大的文献，并确定具体的研究方向。(8. June - 15. June)

  > I will browse and filter literature by different categories, learn about existing research on the topic and make notes, other than that, try to find directions for improvement in existing research. Filter the literature for papers of little relevance to the topic.

- Week4: 根据文献整理结果撰写Project Plan以及文献综述，同时，从各个社交媒体开始爬取文本数据，进行数据收集。(15. June - 22. June)

  > I will write the literature review and project plan based on the organised literature. At the meantime, start crawling text data from various social media for data collection.

- Week5: 修改并整理Project Plan和文献综述， 除了收集社交媒体数据外，仍要收集加密货币相关交易数据。(22. June - 29. June)

  > I will revise and collate the project plan as well as literature review, and collect data on cryptocurrency related transactions in addition to social media data.

- Week6: 对文本数据和加密货币数据进行数据预处理。清理没用的数据并且保证两种数据具有同步性。在数据预处理之后，使用情感分析工具对文本数据进行情感分类并获取sentiment score。(29. June - 6. July)

  > I will perform data pre-processing of text data and cryptocurrency data. Useless data will be cleaned up and simultaneous consistency between the two types of data is ensured. After preprocessing the data, the sentiment analysis tool is used to classify the text data and obtain a sentiment score.

- Week7: 使用统计方法探寻加密货币数据或社交媒体公众情绪之间的关系，并使用有效的指标和深度学习模型对加密货币的价格方向进行预测。(6. July - 13. July)

  > I will use statistical methods to explore the relationship between cryptocurrency data or public sentiment on social media, and use effective metrics and deep learning models to make predictions about the cryptocurrency prices direction.

- Week8: 尝试不同的模型并对加密货币的价格方向进行预测，验证研究的可行性，选择最优的特征方案。(13. July - 20. July)

  > I will experiment with different models and predict the price direction of cryptocurrencies, validate the feasibility of the study, and choose the optimal feature solution for the best performance.

- Week9: 尝试构建行为推荐系统，并开始使用D3.js对研究结果构建交互式可视化。(20.July - 27. July)

  > I will try to build a action recommendation system and start to use D3.js to build interactive visualisations for research results

- Week10: 开始撰写论文，并且同步可视化的构建和模型的修改和完善。(27. July - 3. Aug)

  > I will start to thesis writing and simultaneously perform the visualization construction and model validation and refinement.

- Week11: 撰写论文，并进一步完善研究内容。(3. Aug - 10. Aug)

  > Writing a thesis and further developing the research

- Week12: 润色并修改论文 (10. Aug - 17. Aug)

  > Retouching and revising the paper

- Week13: 润色并完善论文 (17. Aug - 25. Aug)

  > Retouching and perfecting the thesis

- Week14: 最终检查论文并进行提交 (25. Aug - 31. Aug)

  > Final check of the thesis and submission.



## Risk Assessment

> A one-page risk assessment for your project, talking about the major risks you can foresee that might plausibly occur and interfere with your plans. For each risk, state clearly what it is, what its likelihood is, what its effects/impact would be on the project, and what your intended mitigation or risk-reduction involves. 

> **Note that all tables and figures in your document (and in your final thesis) require a number and a caption. In the main text you must refere to every table and figure by number and name. Tables have caption ABOVE the table. Figures have caption BELOW the figure.** 



| Risk                               | Likelihood | Severity | Mitigation                      |
| ---------------------------------- | ---------- | -------- | ------------------------------- |
| Computer Problem(stolen or broken) | Medium     | High     | Backup all work using the cloud |
| 数据无法爬取（API limitations）    | High       | Medium   | 选择其他数据来源或者换用其他API |
|                                    |            |          |                                 |





[^1]: Predicting Fluctuations in Cryptocurrency Transactions Based on User Comments and Replies
[^2]: Understanding Social Factors Affecting The Cryptocurrency Market
[^3]: HOW ARE TWITTER ACTIVITIESRELA TED TOTOP CRYPTOCURRENCIES' PERFORMANCE? EVIDENCE FROM SOCIAL MEDIA NETWORKAND SENTIMENT ANALYSIS
[^4]: Cryptocurrency Price Prediction Using Time Series and Social Sentiment Data
[^5]: Price Movement Prediction of Cryptocurrencies Using Sentiment Analysis and Machine Learning
[^6]: Advanced social media sentiment analysis for short-term cryptocurrency price prediction
[^8]: https://bris.idm.oclc.org/login?qurl=https%3A%2F%2Fwww.sciencedirect.com%2Fscience%2Farticle%2Fpii%2FS0167629602000115%3Fvia%253Dihub
[^9]: Panger, G. T. (2017). Emotion in social media (Doctoral dissertation, UC Berkeley).
[^10]: The predictive power of public Twitter sentiment for forecasting cryptocurrency prices
[^11]: L. Kristoufek. BitCoin meets Google Trends and Wikipedia: Quantifying the relationship between phenomena of the Internet era. Scientific Reports, 3:1–7, 2013.
[^ 12]: Predicting Fluctuations in Cryptocurrency Transactions Based on User Comments and Replies
[^13]: Twitter Sentiment Analysis-Based Adjustment of Cryptocurrency Action Recommendation Model for Profit Maximization
[^14]: D. M. Q. Nelson, A. C. M. Pereira, and R. A. de Oliveira, ‘‘Stock market’s price movement prediction with LSTM neural networks,’’ in Proc. Int. Joint Conf. Neural Netw. (IJCNN), Anchorage, AK, USA, May 2017, pp. 1419–1426.
[^15]: T. Sanboon, K. Keatruangkamala, and S. Jaiyen, ‘‘A deep learning model for predicting buy and sell recommendations in stock exchange of Thailand using long short-term memory,’’ in Proc. IEEE 4th Int. Conf. Comput. Commun. Syst. (ICCCS), Singapore, Feb. 2019, pp. 757–760.
[^16]: J. Park and Y.-S. Seo, ‘‘A deep learning-based action recommendation model for cryptocurrency profit maximization,’’ Electronics, vol. 11, no. 9, p. 1466, May 2022.
[^17]: Nakamoto, S., 2008. Bitcoin: A Peer-to-Peer Electronic Cash System.
[^18]: https://litecoin.org/
[^19]: https://ethereum.org/en/
[^21]: Selgin, G. (2015). Synthetic commodity money. Journal of Financial Stability, 17, 92-99.
[^22]: Empirical Analysis Тowards the Effect of Social Media on Cryptocurrency Price and Volume
[^23]: https://www.sciencedirect.com/science/article/pii/B9780128021170000023
[^24]: Hinman, W., 2018. Digital Asset Transactions: When Howey Met Gary (Plastic), Remarks at the Yahoo Finance All Markets Summit: Crypto.
[^25]: Dyhrberg, A. H. (2016). Hedging capabilities of bitcoin. Is it the virtual gold?. Finance Research Letters, 16, 139-144.
[^26]: Wang, H., Lu, S., & Zhao, J. (2019). Aggregating multiple types of complex data in stock market prediction: A model-independent framework. Knowledge-Based Systems, 164, 193–204.
[^27]: Forecasting Cryptocurrency Returns from Sentiment Signals: An Analysis of BERT Classifiers and Weak Supervision
[^28]: Loginova, E., Tsang, W. K., van Heijningen, G., Kerkhove, L.-P., & Benoit, D. F. (2021). Forecasting directional bitcoin price returns using aspect-based sentiment analysis on online text data. Machine Learning, 1–24.
[^29]: Kraaijeveld, O., & De Smedt, J. (2020). The predictive power of public Twitter sentiment for forecasting cryptocurrency prices. Journal of International Financial Markets, Institutions and Money, 65, 101188.
[^30]: Nasekin, S., & Chen, C. Y.-H. (2020). Deep learning-based cryptocurrency sentiment construction. Digital Finance, 2, 39–67.
[^31]: Pang, B., Lee, L., & others. (2008). Opinion mining and sentiment analysis. Foundations and Trends in Information Retrieval, 2, 1–135.
[^32]: Mishev, K., Gjorgjevikj, A., Vodenska, I., Chitkushev, L. T., & Trajanov, D. (2020). Evaluation of sentiment analysis in finance: From lexicons to transformers. IEEE Access, 8, 131662–131682.
[^33]: Kaplanski, G., Levy, H., 2010. Sentiment and stock prices: the case of aviation disasters. J. Financ. Econ. 95 (2), 174–201. https://www.sciencedirect.com/science/article/pii/S0304405X09002086
[^34]: Liu, B., Zhang, L., 2012. A survey of opinion mining and sentiment analysis. In: Mining Text Data, volume 9781461432, pp. 415–463. Lo, A.W., 2004. The adaptive markets hypothesis: Market efficiency from an evolutionary perspective.
[^35]: M. Hasan, E. Rundensteiner, and E. Agu, ‘‘Automatic emotion detection in text streams by analyzing Twitter data,’’ Int. J. Data Sci. Anal., vol. 7, no. 1, pp. 35–51, Feb. 2019.
[^36]: Şaşmaz, E., and F. B. Tek. 2021. Tweet sentiment analysis for cryptocurrencies. Proceedings of the 6th International Conference on Computer Science and Engineering, 613–18, Ankara, Turkey, 2021 September.
[^37]: Sentiment Analysis and Emotion Detection on Cryptocurrency Related Tweets Using Ensemble LSTM-GRU Model
[^38]: Bi-Directional CNN-RNN Architecture with Group-Wise Enhancement and Attention Mechanisms for Cryptocurrency Sentiment Analysis
[^39]: Prediction of cryptocurrency returns using machine learning
[^40]: Fama, E. F. (1970). Efficient capital markets: A review of theory and empirical work. Journal of Finance, 25, 383–417.
[^41]: Mandelbort, B. (1971). When can price be arbitraged efficiently? A limit to the validity of the random walk and martingale properties. Review of Economic Statistics, 53, 225–236.
[^42]: Urquhart, A. (2016). The inefficiency of Bitcoin. Economics Letters, 148, 80–82. Vidal-Tomas, D., & Ibanez, A. (2018). Semi-strong efficiency of Bitcoin. Finance Research Letters, 27, 259265.
[^43]: Chen, A.; Leung, M.; Daouk, H. Application of Neural Networks to an Emerging Financial Market: Forecasting and Trading the Taiwan Stock Index. Comput. Oper. Res. 2014, 30, 901–923. [CrossRef]
[^44]: Using Time-Series and Sentiment Analysis to detect the Determinants of Bitcoin Prices
[^45]: When Bitcoin encounters information in an online forum: Using text mining to analyse user opinions and predict value fluctuation
[^46]: Panger, G.T.: Emotion in Social Media. PhD thesis, University of California, Berkeley (2017)
[^47]: Cryptocurrency Price Prediction Using Tweet Volumes and Sentiment Analysis



[^48]: Thelwall,M.(2017OnlineFirst).Cansocialnewswebsitespayfor contentandcuration?TheSteemItcryptocurrencymodel. Journalof InformationScience ,1–24.https://doi.org/10.1177/0165551517748290
[^49]: The dynamics of cryptocurrency market behavior: sentiment analysis using Markov chains
[^50]: Smuts N (2019) What drives cryptocurrency prices? An investigation of google trends and telegram sentiment. ACM SIGMETRICS Perform Evalu Rev 46(3):131–134
[^51]: Cryptocurrency trading: a comprehensive survey
[^52]: Global cryptocurrency trend prediction using social media
[^53]: DL-GuesS: Deep Learning and Sentiment Analysis-Based Cryptocurrency Price Prediction
[^54]: A Stacking Ensemble Deep Learning Model for Bitcoin Price Prediction Using Twitter Comments on Bitcoin
[^55]: An Intelligent System for Trading Signal of Cryptocurrency Based on Market Tweets Sentiments







[^92]: Machine Learning the Cryptocurrency Market
[^93]: BitCoin meets Google Trends and Wikipedia: Quantifying the relationship between phenomena of the Internet era
[^94]: Stock Price Prediction Using a Multivariate Multistep LSTM: A Sentiment and Public Engagement Analysis Model
[^95]: Forecasting stock market movements using Google Trend searches
[^96]: What Drives Cryptocurrency Prices?: An Investigation of Google Trends and Telegram Sentiment
[^97]: Predicting Hourly Bitcoin Prices Based on Long Short-Term Memory Neural Networks



[^98]: Bitcoin price prediction using machine learning: An approach to sample dimension engineering
[^99]: Predicting the Price of Bitcoin Using Machine Learning
[^100]: A Comparative Study of Bitcoin Price Prediction Using Deep Learning
[^101]: Bitcoin Price Prediction: An ARIMA Approach
[^102]: Real-world model for bitcoin price prediction
[^103]: Bitcoin price forecasting method based on CNN-LSTM hybrid neural network model
