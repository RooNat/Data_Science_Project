---
typora-copy-images-to: ./attachments
---



# Thesis

[toc]

## Introduction

本章首先介绍了项目研究的动机及主要目标，并说明了为什么探究社交媒体数据与比特币的价格的联系为一个流行的研究领域。接下来，本章说明了研究比特币价格变化预测和利用社交媒体情绪研究对比特币价格影响的挑战。最后，概述了本研究关注的重点问题，以及该项目的具体目标。

本章首先介绍了质量保证系统的背景，然后提出了交互式质量保证的动机，并说明了为什么交互式质量保证是一个重要的研究领域。
交互式质量保证是一个重要的研究领域。接下来，我们说明了以往交互式学习方法的局限性，并介绍了我们提出的
以前的交互式学习方法的局限性，并对我们提出的方法进行说明，同时强调开发过程中的挑战。
发展挑战。最后，概述了该项目的研究目标。



在研究多种社交媒体平台的公众情绪对比特币的价格影响时，会遇到一系列的挑战，其中最大的挑战之一便是数据的来源以及数据的质量，收集不同的社交媒体平台的大规模数据是困难的，且数据的质量可能会收到公众平台虚假信息或者机器人发帖的问题的影响，这些都会对数据添加噪音。除此以外，文本的处理以及情感分析也是一项挑战，因为文本处理经常需要联系上下文，且语言具有多义性，这可能会导致情感分析结果不准确。且社交媒体的公众情绪对比特币的影响可能具有滞后性，即价格变动后可能才会出现情绪变化，或者情绪变化亦会导致价格变动，在构建模型时也需要考虑时间序列的滞后效应。最后，模型的选择也是一项难题，时间序列分析问题需要对数据进行多重处理后才能输入模型，例如，由不平稳数据转换成平稳数据等。模型还需考虑其泛化能力并避免过拟合等问题。

总之，探索多个社交媒体平台的公众情绪与比特币价格波动之间的关系需要克服数据采集、处理、分析和模型选择等多个方面的挑战，同时还需要发掘两者之间的潜在关系。



1. **数据来源和质量：** 获取不同社交媒体平台的大规模数据可能会涉及到数据收集的难题。此外，数据的质量、真实性和准确性可能因为虚假信息、机器人账号等问题而受到影响。
2. **多样性和杂散性：** 社交媒体上的信息非常多样化，包括情感、评论、新闻、谣言等。将这些信息整合并加以处理可能会变得复杂，需要有效的数据清洗和预处理流程。
3. **情感分析的复杂性：** 分析公众情绪需要进行情感分类，而人类语言充满了多义性、隐喻和上下文相关性，这可能导致情感分析的准确性下降。
4. **情感与价格的复杂关系：** 公众情绪与比特币价格之间的关系可能是复杂且多维的，不仅仅是直接的因果关系。其他因素如市场供需、政策变化等也可能影响价格波动。
5. **时序性和滞后效应：** 在社交媒体上观察到的情绪可能有时滞，即情绪的表达可能会在实际价格变动之后才出现。因此，需要解决时序性和滞后效应对分析的影响。
6. **模型选择与泛化：** 使用多种模型算法可能需要权衡选择适合的算法，并考虑其泛化能力和过拟合问题。
7. **数据处理和特征工程：** 整合不同来源的数据、构建有效的特征集合以及处理缺失值等都是需要解决的问题。
8. **解释性和可解释性：** 将模型的结果解释给非技术人员，让其理解情感和价格之间的关系，可能会面临一定的挑战。
9. **市场动态性：** 加密货币市场的特点是高度动态和不稳定的，市场状况可能会在短时间内发生剧烈变化，这可能影响模型的准确性和稳定性。
10. **过度关注和噪音：** 某些事件或话题可能在社交媒体上受到过度关注，而且有大量的噪音存在。这可能干扰情感分析的结果。

## Methodology

本论文遵循了CRISP-DM的流程。什么是CRISP-DM流程？CRIPS-DM流程包含以下方面：

==放图==

本章目的是介绍研究的整体流程架构以及每一步流程的基本方法，整体流程以CRISP-DM流程为基础，本章主要关注于数据的处理，模型的构建，以及动作推荐的策略。模型的评估方法和具体实施将在第四章内提供。

The purpose of this chapter is to introduce the overall framework of the research methodology and the fundamental methods employed in each step of the process. The overall process is built upon the CRISP-DM framework, with a focus on data preprocessing, model construction, and action recommendation strategies. The evaluation methods for the models and their specific implementation will be provided in Chapter 4.

### Architecture

本项目目的是探索短期内社交媒体数据与Bitcoin数据的关系，研究公众情绪对Bitcoin price的滞后影响，并根据此选择特征对比特币价格及其方向进行预测，以此验证两者关系，并利用得到最佳预测结果的模型对下一时刻进行trading action reccommendation，即"Buy","Sell" and "Wait". 公众情绪由sentiment analysis获得，除此之外，还探索了Google Trends数据对Bitcoin Price的短期滞后影响。本研究中的所有数据都基于一小时的时间间隔。最终，对动作推荐结果进行回测，以获得整体的一段时间的total return。其整体流程遵循了CRISP-DM流程，其是一种用于数据挖掘和机器学习项目的流程模型，主要包含**Business  Understanding**， **Data Understanding**，**Data Preparation**，**Modeling**，**Evaluation**,Deployment六步。本研究在此基础上对流程进行了调整，整体架构如图所示。

> The aim of this project is to explore the relationship between short-term social media data and Bitcoin data, investigating the delayed impact of public sentiment on Bitcoin price. Based on this exploration, the project selects features to predict both Bitcoin price and its direction. This is done to validate their relationship. The optimal predictive model is then used to provide trading action recommendations for the next time step, which includes "Buy," "Sell," and "Wait" actions.
>
> Public sentiment is obtained through sentiment analysis, and beyond that, the study also explores the short-term lagging impact of Google Trends data on Bitcoin price. All data in this study is based on hourly intervals. Finally, backtesting is conducted on the trading action recommendations to obtain the overall total return over a period of time.
>
> The entire process adheres to the CRISP-DM methodology, a process model used for data mining and machine learning projects. It consists of six stages: Business Understanding, Data Understanding, Data Preparation, Modeling, Evaluation, and Deployment. This study adapts this methodology to its needs, and the overall architecture is illustrated in the accompanying diagram."

### Data Collection

为了研究媒体数据与bitcoin数据的关系以及媒体数据对每小时bitcoin价格的预测能力，除比特币历史价格数据外，多个不同的数据源被考虑作为可能的特征。第一个考虑的特征是比特币相关推文的sentiment score和推文数量。第二个是Reddit的比特币子版块的Posts，第三个是谷歌趋势数据。本节详细介绍了如何收集各类数据。

> In order to explore the potential correlation between media-related information and Bitcoin trends, as well as to evaluate the capacity of media data to predict hourly Bitcoin price fluctuations, a comprehensive investigation incorporated multiple diverse data sources beyond historical Bitcoin pricing data.
>
> Three key variables were under scrutiny. The first variable encompassed the sentiment scores and tweet volume associated with Bitcoin-related tweets on Twitter. The second variable involved an examination of posts within the Bitcoin-focused subreddit on Reddit. Lastly, the third variable entailed the integration of data derived from Google Trends.
>
> This section provides an in-depth exposition of the methodologies adopted for the collection of this array of diverse data.

#### Financial Data

与大多数其他针对加密货币的研究不同的是，比特币在此项目中没有按天来收集，而是以小时为时间间隔进行收集。比特币历史价格数据是从Binance的历史数据API获取的，Bianace是一个加密货币交易平台，允许用户购买、出售和交易各种加密货币, 即binance_historical_data[^56] API, 此API从Binance的服务器上下载历史加密货币。

https://pypi.org/project/binance-historical-data

> In contrast to the majority of cryptocurrency studies, this project diverges by gathering Bitcoin data not on a daily basis but at hourly intervals. The historical price data for Bitcoin was sourced from Binance's historical data API. Binance serves as a cryptocurrency trading platform where users can engage in the buying, selling, and trading of a diverse range of cryptocurrencies. Specifically, the binance_historical_data[^56]API was utilized to access historical cryptocurrency data stored on Binance's servers.

BTCUSDT 是一个交易量很大的热门交易货币对。许多交易者和投资者都在积极交易该货币对，从而产生了大量的交易和数据点。USDT是一种稳定币，旨在与美元保持 1:1 的挂钩，由于 BTCUSDT 交易对的流行性和稳定性，它经常被用于市场分析、技术分析和价格预测。因此本项目将获取bitcoin与USDT的交易对历史价格数据，下载的数据范围是2022年1月1日到2022年6月30日，本研究将关注2022年的前半年的hourly数据，即按小时获取比特币的历史价格和交易量。最终下载的数据将按照月份分别以.csv格式存储在本地文件夹中。从API获取的原始数据的数据格式如表所示：

> The BTCUSDT currency pair is widely traded with substantial volume, attracting active traders and investors and generating a wealth of trading data. USDT is a stablecoin pegged to the U.S. Dollar. Due to the popularity and stability of the BTCUSDT pair, it's commonly used for market and technical analysis. This project aims to gather historical bitcoin price data in USDT from 1st January to 30th June, 2022. The focus is on hourly data for the first half of 2022, capturing price and volume trends.The collected data will be stored as separate monthly ".csv" files locally. The raw data format obtained from the API is illustrated in a table.

| Open time     | Open     | High     | Low      | Close    | Volume   | Close time    | Quote asset volume | Number of trades | Taker buy base asset volume | Taker buy quote asset volume |      |
| ------------- | -------- | -------- | -------- | -------- | -------- | ------------- | ------------------ | ---------------- | --------------------------- | ---------------------------- | ---- |
| 1640995200000 | 46210.57 | 46729.73 | 46210.55 | 46650.01 | 8957.465 | 1640998799999 | 416444814.3        | 91267            | 4777.701                    | 222129624.4                  |      |

该数据包含了12列，其中'Open time'和'Close time'是交易周期（K线图）开始和结束时的时间戳。它标记了特定时间间隔的起始时间和结束时间，‘Open’,'Close'分别表示交易周期开始和结束时的价格，'High', 'Low'分别表示交易周期内加密货币的最高价格和最低价格。'Volume'表示交易周期内加密货币的总交易量。在本研究中，特定时间间隔为一小时。

'Quote asset volume', 'Number of trades', 'Taker buy base asset volume'和'Taker buy quote asset volume' 提供了对交易活动各个方面的深入了解，包括交易量、交易次数以及积极接受市场价格的交易者（做市商）的行为，但是对本项目没有意义，因此在处理数据时，这些列将被移除，在此环节最终保留的。

#### Social Media Data

社交媒体数据将从两类最受欢迎且日流量较大的社交媒体平台获得，分别是Twitter和Reddit。

（Twitter和Reddit的介绍）

本小节介绍社交媒体数据的获取方法

> Social media data will be obtained from the two most popular and high daily traffic social media platforms, Twitter and Reddit. This subsection describes the acquisition of social media data

##### Tweets Data

> Due to the limitations of Twitter's official API, in this project, tweets data was sourced from Kaggle's publicly available historical dataset[^57] which leveraged the Snscrape Python package to collect more than 22 million English-language tweets mentioning the word "Bitcoin" over the time period from 1 January 2021 to 30 June 2022. The dataset is divided into two files: one containing 15.6 million tweets from 2021 and the other comprising 8.17 million tweets from the first half of 2022. This study is exclusively concerned with the data from January to June 2022. The table illustrates sample data, with the username listed as anonymous.

由于Twitter官方API具有限制，在这个项目中，Twitter的文本数据来源于Kaggle已公开的历史数据集[^57]，该数据集使用Snscrape Python包收集了2200 多万条提及 "比特币 "一词的英文推文，时间范围从2021年1月1日到2022年6月30日。数据集分为两个文件，分别包含2021 年的 1560 万条推文和2022 年上半年的 817 万条推文。此研究只关注2022年的1到6月的数据。其原数据包含四列，分别为"datetime","date", "username" and "text"。表格为样本数据，其username为匿名：

| datetime                  | date       | username  | Text                                                         |
| ------------------------- | ---------- | --------- | ------------------------------------------------------------ |
| 2022-01-01 23:59:56+00:00 | 2022-01-01 | Username1 | 0.4MOT TOKENS IN #LATOKEN airdrop and maybe more! #Bitcoin and #Cryptocurrency |
| 2022-01-01 23:59:53+00:00 | 2022-01-01 | Username2 | MARA for Bitcoin Exposure: Top Trade Q1 2022 https://t.co/EZ2S4cfFIq https://t.co/srsvNPA2jV |

##### Reddit Post Data

> The second source of social media data comes from Reddit, with a particular focus on cryptocurrency-related and Bitcoin-related subsections. Initially, the plan was to directly collect data using the Reddit API and a Python crawler tool. However, the official Reddit API offers only around 1,000 data points per hour for regular users, falling significantly short of meeting the research needs. As a result, the ultimate source of Reddit data came from Kaggle, which provides a publicly accessible historical dataset of Reddit posts related to cryptocurrencies. This dataset spans from January 1st, 2022 to December 30th, 2022. The data was compiled by utilizing a combination of a crawler and the pushshift API to gather Reddit submission IDs, followed by the Python Reddit API to retrieve detailed submission information.
>
> Given the diverse landscape of cryptocurrency-related discussions on Reddit, this study specifically concentrates on the 'r/bitcoin', 'r/btc', and 'r/cryptocurrency' boards. These subsections collectively account for 243,187 posts in the 'r/cryptocurrency' board, and 66,576 and 21,353 posts in the 'r/bitcoin' and 'r/btc' boards, respectively. Each dataset encompasses 25 columns, capturing a comprehensive range of attributes.

第二个社交媒体的数据源是Reddit。主要关注加密货币及比特币相关的Reddit子版块。在研究的初阶段，打算使用Reddit API和Python抓取工具直接进行数据收集。但是Reddit官方API每小时只向普通用户提供大约1000条数据，远远不能满足研究目的。因此，最终Reddit数据同样从Kaggle平台获取，其提供了加密货币相关Reddit Posts的公开历史数据集[^58]。该数据使用爬虫和 pushshift API 收集 reddit submission id，并使用 python reddit API 获取submission details，数据范围为2022年1月1日到2022年12月30日。加密货币相关的Reddit子版块有许多，基于研究目的，本数据集只重点关注比特币子版块及加密货币子版块的数据，即来源于'r/bitcoin', 'r/btc' and 'r/cryptocurrency'板块的posts。其中'r/cryptocurrency' 板块包含243187数据，'r/bitcoin'和'r/btc'分别包含66576和21353条数据，每条数据包含25列，原数据的样本数据见表：

| submission | subreddit      | author        | created    | retrieved  | edited | pinned | archived | locked | removed | deleted | is_self | is_video | is_original_content | title                                      | link_flair_text | upvote_ratio | score | gilded | total_awards_received | num_comments | num_crossposts | selftext                                                     | thumbnail | shortlink              |
| ---------- | -------------- | ------------- | ---------- | ---------- | ------ | ------ | -------- | ------ | ------- | ------- | ------- | -------- | ------------------- | ------------------------------------------ | --------------- | ------------ | ----- | ------ | --------------------- | ------------ | -------------- | ------------------------------------------------------------ | --------- | ---------------------- |
| rt6orv     | cryptocurrency | AutoModerator | 1640995213 | 1641139644 | 0      | 0      | 0        | 0      | 0       | 0       | 1       | 0        | 0                   | Daily Discussion - January 1, 2022 (GMT+0) | OFFICIAL        | 0.92         | 199   | 0      | 24                    | 8489         | 0              | **Welcome to the Daily Discussion. Please read the disclaimer, guidelines, and rules before participating.**   .... | self      | https://redd.it/rt6orv |

#### Google Trends Data

Google 趋势数据提供有关特定搜索词在任何给定时间相对于其他搜索词的流行程度的信息。此外，可以随时间比较这些搜索词流行度值。这为任何给定时间对加密货币的普遍兴趣提供了一个代理指标，随着时间的推移，随着普遍兴趣的增加和减少，这可能与加密货币价格存在关系[^59]。

Google Trends 是一项Google 提供的搜索趋势功能，它不提供具体的搜索量，而是提供搜索量指数(SVI)[^59]。搜索量指数是给定时间段和给定地理区域的每个时间点相对于整个时间段总搜索量的频率[^60]，这些数字会缩放到 0 到 100 之间，根据搜索词与所有搜索词的比例[^59]。当查询超过 90 天的趋势数据时，返回的 SVI 会按周进行汇总[^59]。使用Pytrends API以周为单位获取以Bitcoin为搜索关键词的Google Trends数据，整体的数据时间范围为2022年12月24日到2022年7月1日，数据时间间隔为每小时。由于数据是以周为单位进行缩放的，为获取当前数据点相对于整个时间范围的搜索量指数，还需要后续的处理。

> Google Trends data furnishes insights into the relative popularity of specific search terms compared to others, offering a temporal dimension to this comparison. This dynamic lends itself to serve as a proxy indicator for the prevailing level of general interest in cryptocurrencies at any given juncture. Such interest fluctuations might potentially correlate with cryptocurrency price movements, as interest levels invariably rise and fall[^59].
>
> Google Trends, a service by Google, provides trends in search terms without yielding precise search volume. Instead, it offers a Search Volume Index (SVI)[^59]. The SVI gauges the frequency of total searches for a given time frame and geographic region, relative to the overall searches for that time period[^60]. These values are then normalized to a scale between 0 and 100, factoring in the proportion of the search term to all search terms[^59]. For trend data spanning over 90 days, SVIs are presented on a weekly aggregation[^59].
>
> In this study, the Pytrends API was harnessed to gather Google Trends data, focusing on the search term "Bitcoin" on a weekly basis. The data encompasses the interval from December 24th, 2022 to July 1st, 2022, with hourly data increments. Since the data is inherently weekly-scaled, subsequent manipulation is requisite to derive the Search Volume Index for each data point, taking into account the broader timeframe.

---

### Data Preparation

#### Financial Data

原始Bitcoin价格数据的时间以时间戳形式被记录，时间戳是一串数字。为进行数据分析，需要对Bitcoin数据进行进一步处理，在Python中使用datetime包将时间戳数据转换为真实的时间以便与其他数据源进行合并。去除'Quote asset volume', 'Number of trades', 'Taker buy base asset volume'和'Taker buy quote asset volume' 等无用列，并将“Open time"作为每一组数据的唯一时间点。使用for循环读取文件夹内的每月数据并进行合并，合并后的数据日期并不连续，使用yfinance包从Yahoo Finance[^62]获取缺失日期的BTCUSDT数据进行填补。

> The initial Bitcoin price data uses numeric timestamps. To integrate it with other sources, the `datetime` package in Python converts these timestamps into datetime. Unnecessary columns like 'Quote asset volume', 'Number of trades', 'Taker buy base asset volume', and 'Taker buy quote asset volume' are removed.  The 'Open time' column is chosen as the unique key in time for each dataset.  Data from different sources is merged using a `for` loop. Temporal gaps are resolved by leveraging the `yfinance` package to retrieve missing bitcoin price data from Yahoo Finance[^62].
>
> 



#### Social Media Data

##### Pre-processing

在研究社交媒体数据之前，需要对原始数据进行预处理，本小节阐述推文数据和Reddit Post的原始数据的初步处理方法，以便为后续的情感分析做准备。

> Prior to delving into social media data analysis, a preliminary step involves pre-processing the raw data. This subsection outlines the initial processing applied to both tweet data and Reddit posts. This preparation paves the way for subsequent sentiment analysis.

对于推特数据，需要将日期格式标准化为“%Y-%m-%d %H:%M”，将变量重命名，去除无用的列“date"，只保留具体时间变量。在原始数据中，每条推文对应一个具体时间点。为了与Bitcoin的历史价格数据的时间点相对应，在初步整理过程中，将推文按小时进行分组，将具体时间点舍去，获得以小时为单位的rounded time。在此基础上，可以按小时分组并计算每小时发布的比特币相关推文数量。

> For tweets data, it is essential to normalise the date format to "%Y-%m-%d %H:%M". Variables are renamed, and the redundant "date" column is discarded. Each tweet in the raw data corresponds to a distinct time point. To align with Bitcoin's historical price data, tweets are initially sorted and grouped by the hour. Time points are rounded to the nearest hour for effective synchronization. This process facilitates hourly grouping and counting of Bitcoin-related tweets.

Reddit数据中包含三个'r/cryptocurrency','r/btc','r/bitcoin'三个子版块，对于每个板块，从原始的Reddit Posts数据中提取出2022年1月1日到6月30日的子数据集，并将日期进行与推文数据相同的标准化。Reddit Posts的数据包含25个关键字，大部分的变量对于后续分析没有意义，因此只保留可能对于情绪分数产生影响的变量。与推文数据不同的是，获取的Reddit posts数据中的每一行并不对应一个实际存在的帖子，而是一次提交记录，需要去除帖子为空值的记录。最终将三个子版块的数据进行合并，得到初步处理后的Reddit Posts数据 Reddit Posts数据初步整理后保留的变量及其描述如表格所示：

| Key          | Description                        |
| ------------ | ---------------------------------- |
| datetime     | 帖子创建的时间点（已经按小时分组） |
| upvote_ratio | 对帖子的所有投票中获得的高票百分比 |
| selftext     | 文本帖子的内容                     |



##### Text Clean

文本清理是情绪分析的一个关键性的预处理步骤。社交媒体的原始文本数据经常包含各种噪音，如特殊字符、标点符号和格式问题。整理可以帮助去除这些不必要的元素，有助于去除带有偏见的语言、和冒犯性内容，从而实现无偏的分析。Twitter数据短小并且具有高噪音，因此需要进行大量的文本清理以便进行后续的文本分析。首先，经常发推特的用户被视为噪音，因为他们发布的大多数推特为广告，根据Hirad的方法[^63]，根据用户名筛选并保留发布推特少于1000条的用户，推文总量从8,173,354条降至5,711,375条。进一步，根据Hirad提供的比特币相关的噪音词列表[^63]去除噪音词。此后，从推文中删除 URL、html tags，并扩展标记的缩写(e.g. ‘let's” into ‘‘let us’”)。推特文本是不规则，含有大量俚语或缩写词(e.g. "idk" or "iow")，其会对文本添加噪音，因此使用手动创建的网络俚语列表对文本进行扩展[^64]。通过去除文本的标点符号，数字和多余的空格来进行进一步的标记化和并使用WordNet Lemmatizer应用词形还原。最后，根据NLTK Python库提供的停用词词典去除英文停用词并去除其他non-letter tokens。对所有tokens进行大小写折叠获取最终的tokens。直方图统计被应用到预处理后的数据，统计结果显示大部分文本的长度在0到40之间，长度小于6的推文将被省略，因为它们不适合句子级情感分析[^64]. 经过去重处理后最终的推文数量为3,324,865条。

> 
> Text cleaning is a crucial preprocessing step for sentiment analysis. Raw text data extracted from social media often contains noise such as special characters, punctuation, and formatting issues. Cleaning helps eliminate these undesired elements, mitigating biased language and offensive content to ensure an impartial analysis.
>
> Twitter data is characterized by its brevity and high level of noise, necessitating extensive text cleaning for subsequent analysis. Initially, accounts that frequently post tweets are considered noisy, as a substantial portion of their content comprises advertisements. Following Hirad's approach [^63], the total tweet count was reduced from 8,173,354 to 5,711,375 by filtering based on usernames and retaining users who posted fewer than 1,000 tweets. Additionally, a list of Bitcoin-related noise words [^63] provided by Hirad was utilized to remove noise words. Subsequently, URLs and HTML tags were stripped from tweets, and tagged abbreviations were expanded (e.g., "let's" into "let us"). Twitter text often includes slang and acronyms (e.g., "idk" or "iow"), which introduces noise. To address this, a handcrafted list of internet slang [^64] was employed to expand text. Further tokenization and lexical form reduction involved removing punctuation, numbers, and redundant spaces using the WordNet Lemmatizer. English stopwords were then eliminated, and other non-letter tokens were removed based on the stopword dictionary provided by the NLTK Python library. Case folding was applied to all tokens to achieve uniformity.
>
> Histogram analysis was conducted on the preprocessed data, revealing that most texts ranged between 0 and 40 characters in length. Tweets with lengths less than 6 were excluded from sentence-level sentiment analysis[^64]. After deduplication, the final count of tweets is 3,324,865.

对Reddit Posts应用与推文相同的文本预处理steps，需要注意的是，Reddit posts大部分为长文本，根据直方图统计结果，大部分的posts的长度在0到1000之间，因此保留posts长度在7到1000之间的数据，防止句子长度太短而无法获取sentiment score。处理后的posts数量由31,118条降至29,023条。

> Apply the same text preprocessing steps to Reddit Posts as for tweets. It's important to note that Reddit posts are generally longer texts. According to histogram statistics, most posts have lengths between 0 and 1,000. To ensure meaningful sentiment analysis, posts with lengths between 7 and 1,000 are retained. This prevents excessively short sentences that can impact sentiment scoring. After processing, the number of posts is reduced from 31,118 to 29,023.

##### Sentiment Analysis

情绪分析是指提取通过消息传达的观点或“情绪”[^65]。通过情感分析得到的sentiment score可以被用于研究与Bitcoin历史价格数据的关系。

情感分析通常有两种方法，即机器学习和基于词典，基于机器学习的情感分析可以对文本进行分类，但所有方法都是用带有观点词的情感词典并将其与数据进行匹配以测量极性。本研究倾向于使用基于词典的情感分析，即Vader和Textblob工具。之所以使用 VADER 和 TextBlob 等情感分析工具，是因为它们适合捕捉社交媒体内容中普遍存在的情感细微差别，尤其是在加密货币相关讨论的背景下。Valence Aware Dictionary and sEntiment Reasoner（VADER）是一种基于词典和规则的情感分析模型，专门为社交媒体文本定制。它善于识别 Twitter 等平台上常见的简短和非正式文本中的情感。此外，正如 Kim 等人[^66] 所证明的，VADER 还能为预测加密货币的价格波动提供准确的极性分数。

另一方面，TextBlob 是本研究采用的另一种情感分析工具。TextBlob 为每条推文提供两个属性：极性和主观性。极性得分范围在-1.0 到 1.0 之间，负分表示负面情绪，正分表示正面情绪，0.0 左右表示中立。TextBlob 基于词典的预训练方法使其无需额外预处理即可分析情感。

> 
> Sentiment analysis involves extracting opinions or emotions conveyed through messages[^65]. The resulting sentiment scores can be studied in relation to historical Bitcoin price data.
>
> Sentiment analysis is typically conducted through two approaches: machine learning and lexicon-based. Machine learning-based sentiment analysis classifies text, but both methods employ a sentiment lexicon containing opinion words to gauge polarity. This study leans towards dictionary-based sentiment analysis, utilizing tools like Vader and TextBlob. These tools are chosen for their ability to capture sentiment nuances prevalent in social media content, especially concerning cryptocurrency-related discussions.
>
> Valence Aware Dictionary and sEntiment Reasoner (VADER) is a lexicon and rule-based sentiment analysis model designed for social media text. It excels in identifying sentiment in short, informal texts often found on platforms like Twitter. Additionally, as demonstrated by Kim et al.[^66], VADER provides accurate polarity scores for predicting cryptocurrency price fluctuations.
>
> Conversely, TextBlob, another sentiment analysis tool employed in this study, offers two attributes per tweet: polarity and subjectivity. Polarity scores range from -1.0 to 1.0, where negative values indicate negative sentiment, positive values indicate positive sentiment, and neutrality is centered around 0.0. TextBlob's lexicon-based pre-training methodology allows sentiment analysis without requiring additional preprocessing.

此外Loughran 和 McDonald 金融语料库[^67]也被尝试使用并进行测试，与Vader返回的情感分数结果不同，Loughran 和 McDonald 金融语料库在Pysentiment2库内构建，返回的是一个包含Positive和Negative tokens数量的词典，为计算sentiment score, 需使用公式对分数进行标准化[^78], 公式内P和N分别代表每条文本Postive和Negative tokens的数量：

> Furthermore, the Loughran and McDonald Financial Corpus\footnote{https://sraf.nd.edu/loughranmcdonald-master-dictionary/} was also explored and tested. Unlike VADER's sentiment score results, this corpus was incorporated into the Pysentiment2 library, generating a dictionary with counts of Positive and Negative tokens. To calculate the sentiment score, normalization is applied using the formula \cite{78}, where P and N represent the counts of Positive and Negative tokens per text, respectively

$$
\text{sentiment score} = \dfrac{\text{P - N}}{\text{P + N}}
$$
虽然存在其他情感分析模型，但 VADER 和 TextBlob 在处理加密货币领域的推文和新闻文章中普遍存在的独特语言特点和情感表达方面具有很强的鲁棒性，因此脱颖而出。通过应用这些工具，研究旨在发现 Twitter 和谷歌新闻等平台上表达的情感与由此对加密货币市场动态产生的影响之间的相关性。

对每条推文应用情感分析技术，会得到相应的compound score, 和极性分数。将所有得到的分数按小时分组并取平均值，以获取每小时的平均推文数据的sentment score。Reddit的相关posts包含相应的upvote rate，提供了每条帖子对应的赞同率，考虑通过赞同率调整compound score并获取最终的post分数。如Formula所示：

$$
\text{weighted sentiment score}=\dfrac{\sum_{i=1}^{n}u_{i}c_i}{\sum_{i=1}^nu_{i}}
$$
其中$u_i$是一小时内第i条文本的upvote ratio，$c_i$是第i条文本的compound score, 通过对每条post的sentiment score以小时为单位加权取平均，获取调整后最终得到的sentiment score。

#### Google Trends

为了获取Google Trends的正确的缩放比例，Erik Johansson提出了一种方法，从每周和每日数据创建每日搜索量数据[^61]。本节将参考其方法，利用每小时和每周数据创建每小时的搜索量数据。所收集的Google Trends数据按周存放在列表内，且每周的最后一个小时时间点与下一周的第一个时间点重合。创建一个for循环，将重合的时间点的对应的数据点进行重叠以获取第一周最后一个小时与下一周的第一个小时的SVI之间的比率，并创建一个列表存储比率。该比率将作为校正参数应用于下一周的每个时间点对应的值。之后有两种处理数据的方法：

> To accurately derive Google Trends scaling ratios, Erik Johansson's approach was employed to convert weekly and daily data into daily search volume data \cite{61}. The Google Trends data was organized in weekly lists, ensuring alignment between the final hourly time point of one week and the initial time point of the subsequent week. Ratios for data at overlapping time points were computed by iterating through the list, and these ratios were retained as correction parameters. Two different strategies were employed for processing the data

1. 在一个for循环内，计算出比率后，对下一周的每个时间点的SVI应用其比率，得出校正后的值，并将校正后的值替换原有值。在求第二周最后一个小时的数据与第三周一个小时的数据的比率时，第二周所应用的数据将是校正后的值，如图1所示。
2. 在第一个for循环内求得所有相邻两周的比率，并存储在新的列表内。并将比率应用于原有列表的每一周，第一周除外，因为第一周没有上一周的最后一小时数据。得到的校正后的数据不替换原有值，如图2所示。

> Within a loop, the correction ratios were employed to adjust the data for each time point in the following week, replacing the original values. When calculating the ratio between the last hour of the second week and the first hour of the third week, the corrected values from the second week were utilized.
>
> Ratios for the consecutive two weeks were calculated within the loop and stored in a fresh list. These ratios were then applied to each week in the original list, except the first week, which lacked the last hour's data from the previous week. The corrected data didn't replace the original values.

将收集的Google Trends数据分别进行上述两种方式的处理。由于处理后的数据不在0到100之间的范围内，数据需要进一步缩放到0到100之间。最后将两种数据分别存储到两个文件内。

> The corrected data might not fall within the range of 0 to 100, necessitating further scaling. Ultimately, the data obtained through the two processing methods were saved in separate files.

图3，转换后的plot。



---

###  Correlations

### Spearman correlation

在初步相关性分析中，Shapiro-Wilk test[^68]被应用以进行每个特征的正态分布性检验。Shapiro-Wilk检验是一种假设检验，用于评估数据集是否呈正态分布。它检查数据是否像正态分布一样分布。如果Shapiro-Wilk检验的p值较大（通常大于0.05），表示数据集较可能呈正态分布；如果p值较小，则表示数据集不太像正态分布[^68]。经过测试发现，数据中的各个特征值均不呈现标准正态分布，为了找到调整后的社交媒体特征与比特币收盘价之间的单调关系，对构建的数据集初步采用Spearman相关系数进行相关性分析。Spearman相关性是一种衡量变量之间单调关系的统计方法。它用于分析两个变量之间的趋势，不要求变量呈线性关系，适用于非线性或顺序性关系。Spearman相关系数的值在-1到+1之间，表示两变量的单调趋势程度，正值意味着随着一个变量增加，另一个变量也趋于增加；负值则表示一个变量增加时，另一个变量趋于减少[^69]。接近-1或+1的值表示更强的关联。因此，Spearman相关性分析能够捕捉到变量间的非线性关系和顺序性关联，帮助了解变量之间的趋势[^70]。但是Spearman相关性系数并未考虑时间序列因素，它仅测量关联而不能确定是否两变量之间是否存在因果关系，也不能处理滞后效应。

（是否需要添加公式）？

#### Granger causality testing

Granger Causality 测试是一种在时间序列分析中使用的统计工具，旨在研究两个变量之间潜在的因果关系。它旨在确定一个变量的过去值是否为预测另一个变量未来值提供了统计显著的信息[^71]。假设$X_t$和$Y_t$是两个时间序列，Granger Causality testing可以用于研究$Y_t$是否对$X_t$具有预测能力。在实践中，Granger Causality 测试涉及几个步骤。首先，所研究的时间序列需要是平稳的，以确保结果的可靠性。这时就需要用到增广迪基-福勒（ADF）检验[^73]。ADF 检验评估时间序列数据的平稳性，这对于有效的 Granger Causality 测试至关重要。在确认平稳性后，需要确定滞后阶数。这涉及选择一定数量的过去时间点（滞后）来评估因果关系中的潜在时间延迟。随后，定义零假设为$Y_t$不会导致$X_t$，并应用F检验统计测试，来评估是否包括$Y_t$的滞后值显著提高了预测$X_t$的能力，如果至少一个滞后的 p 值小于显着性水平 (一般设置为0.05)，则拒绝零假设。时间序列$X_t$的自回归模型和通过增加$Y_t$的滞后值来增强的模型方程如下所示[^72]：

> Firstly, the time series under scrutiny must exhibit stationarity to ensure reliable results. To establish this, the Augmented Dickey-Fuller (ADF) test is employed[^73]. The ADF test evaluates the stationarity of time series data, a crucial prerequisite for a valid Granger Causality test. Once stationarity is confirmed, the lag order is determined. This entails selecting a specific number of past time points (lags) to assess potential time delays in causality.
>
> Next, the null hypothesis posits that $Y_t$ does not precede $X_t$. The F-test statistical test is applied to evaluate whether incorporating lagged values of $Y_t$ significantly enhances the predictive capacity for $X_t$. The null hypothesis is rejected if the p-value for at least one lag is below the significance level (usually 0.05). The autoregressive model for time series $X_t$ and the model equation expanded with increased lagged values of $Y_t$ are illustrated below[^72]:” 

X t = α + 𝛾 1 X 𝑡−1 + 𝛾 2 X 𝑡−2 + ⋯ + 𝛾 𝑝 X 𝑡−𝑝   

X t = α + 𝛾 1 X 𝑡−1 + 𝛾 2 X 𝑡−2 + ⋯ + 𝛾 𝑝 X 𝑡−𝑝 + α 1 Y t-1 + ⋯ + α 𝑝 Y t-p 

此外，需要特别注意的是，Granger Causality testing应用的前提条件为两个变量均为时间平稳序列，如果有一个或者两个变量为非平稳的，则可能导致虚假关系[^64]，从而是Granger Causality testing结果无效。在探索社交媒体信息与比特币价格之间的关系时，需要先判定两列变量是否都为平稳序列，非平稳序列需要通过差分转换成平稳序列并进行进一步测试。

---

### Data Pre-processing

在准备好社交媒体等公众情绪数据和Bitcoin历史价格数据后，以时间为关键字合并数据集，从而进一步构建用于预测价格的特征集和Targets，本小节介绍在将数据输入模型前需对数据进行的进一步处理操作，以及对比特币价格数据的三种预测类型。

> After preparing the public sentiment data, which includes social media information and historical Bitcoin prices, the datasets are merged based on time points. This generates sets of features and targets for price prediction. This section explains the essential steps to process the data for inputting into the model, along with the three types of predictions for Bitcoin price data.

#### Initial Feature Set

利用社交媒体情绪得分，谷歌趋势数据，金融数据构建初始数据集，将所有特征以时间点为关键字进行合并，其中社交媒体数据在某些时间点上缺失情绪得分，在这种情况下通常有两种处理方法：直接去除数据缺失行，或者假设情绪是可延续的，并使用上一个时间点的情绪得分填补缺失行。在初始阶段对两种都进行了尝试，最终选择使用情绪得分对空缺值进行填补，这是因为直接去除数据缺失行会删除整个时间点的数据，从而使得时间不连续，可能会对实验效果产生影响。得到的初始特征集如表所示，初始特征集是所有待研究的特征的集合，在后续过程中需要使用特征子集来测试单一变量对结果的影响。

> An initial dataset was formed by merging social media sentiment scores, Google Trends data, and financial information based on timestamps. Dealing with missing sentiment scores in social media data, two methods were tested: directly removing affected rows or filling gaps with scores from the previous time point assuming contiguous sentiment. The latter was chosen to maintain time continuity and experiment effectiveness. The initial feature set, displayed in the table, contains all studied features. A subset will be used in subsequent analysis to assess variable impact.



> An initial dataset was created by combining social media sentiment scores, Google Trends data, and financial information. All features were merged based on timestamps. The social media data occasionally had missing sentiment scores at specific time points. To deal with this, two approaches were explored: either directly removing rows with missing data or assuming contiguous sentiment and filling gaps using scores from the previous time point. Both methods were tested initially, and the final decision was to fill missing values with sentiment scores. This choice was made to maintain continuity in time, as removing rows disrupts the sequence and could impact experiment effectiveness. The initial feature set is displayed in the table, encompassing all studied features. A subset of these features will be utilized in subsequent analysis to assess the individual impact of variables.

|      |      |      |
| ---- | ---- | ---- |
|      |      |      |
|      |      |      |
|      |      |      |

#### Targets

> This study examines using social media data for three types of Bitcoin price prediction: Bitcoin closing price, price return, and price movement direction. In the cryptocurrency field, predicting Bitcoin's price is a common challenge, and initial efforts focus on forecasting the hourly closing price using models. However, due to Bitcoin's volatile nature, directly predicting its price is impractical, especially given the irregularity of financial time series[^84]. Non-stationary time series data is hard to predict using linear models but can be addressed using deep models with consistent price ranges in training and test sets.
>
> In trading, emphasis often shifts to price returns and movement direction rather than exact prices. Price return time series captures these dynamics as the difference in closing prices over time, calculated as:

本文研究了利用社交媒体数据对三种比特币价格预测类型的问题，即比特币的收盘价预测，价格回报预测，和价格移动方向预测。在加密货币领域，比特币的价格预测是大部分相关项目考虑的问题，在项目初期，尝试使用模型对比特币每小时的收盘价进行预测，此时收盘价是模型的targets。但是比特币的价格以震荡幅度大而闻名，直接预测比特币的价格不具有实际意义，因为金融时间序列很少是平稳的[^84]，非平稳的时间序列数据很难在线性模型内预测，在训练集与测试集的价格范围一致的情况下，可以在深度模型内预测。尽管如此，实际交易中，人们更加关注价格回报和价格走势而非实际价格，价格回报时间序列是对收盘价的差分，其计算公式为：
$$
r_{t,t-k}=\dfrac{p_{t}}{p_{t-k}}-1
$$
其中$p_t$定义为bitcoin在时间t时的价格，$k$表示计算回报的周期数[^82].在本文中，k=1,表示计算当前时间相对前一时间点的价格水平的变化。

> Where $p_t$ represents the bitcoin price at time t, and $k$ indicates the number of cycles for return calculation[^82]. In this study, when $k=1$, it signifies assessing the price change at the current time compared to the previous time point.

价格移动方向预测是一个二分类问题，为数据集中的每条数据分配一个label $y$, 其中label定义如下：
$$
y_t=\begin{cases}
1 & \quad p_{t-1}<p_{t}\\
0 & \quad \text{otherwise}
\end{cases}
$$
其中"1" 代表时间点t相比前一时间点价格上涨[^83]，在这种情况下一般在t-1时刻对资产做多，即进行“买入”操作，"0"则代表持平或价格下跌，此时一般进行做空操作，即“卖出”。



---

### Models

本研究主要使用了五种模型，其中ARIMA模型和Random Forest Classifier分别为价格预测的回归和分类问题的基线模型。深度学习模型主要用于价格走势预测。

> This study employs five models, including the ARIMA model as the baseline for regression in price prediction and the Random Forest Classifier as the baseline for classification. The deep learning model is primarily utilized for predicting price trends.

#### Baseline Models

> *Stock market's price movement prediction with LSTM neural networks*

1. ARIMA Model

   自回归综合移动平均线 (ARIMA) 是一种时间序列预测方法[^88]，旨在预测时间序列数据的未来趋势，它在预测过程中忽略了自变量，适用于关联性强的统计数据，需要满足自相关、趋势和季节性等假设。该方法可以预测复杂的数据影响下的历史数据，特别是在短期内预测的准确度较高，并且能够有效处理季节性波动。ARIMA 方法主要分为四组模型：自回归（AR）、移动平均（MA）、自回归移动平均（ARMA）和自回归综合移动平均（ARIMA）[^74]。自回归 (AR) 模型关注当前数据与前一时期数据的关系，通过自回归阶数 p 来表示。移动平均 (MA) 模型关注误差项与前一时期的误差项的关系，通过移动平均阶数 q 来表示。ARMA 模型是 AR 和 MA 模型的结合体，同时考虑上一时期数据和误差项的影响，其公式如下。
   $$
   \begin{align}
       Y_t = c + \phi_1 Y_{t-1} + \phi_2 Y_{t-2} + \cdots + \phi_p Y_{t-p} +\\ 
     \varepsilon_t - \theta_1 \varepsilon_{t-1} - \theta_2 \varepsilon_{t-2} - \cdots - \theta_q \varepsilon_{t-q} \label{eq}
   \end{align}
   $$
   where $\varepsilon_t$ is white noise, and c is the constant.此方程还可解释为：预测 = 常数 + Y 的线性组合滞后 + 滞后预测误差的线性组合[^90]

   然而，ARIMA 模型对数据有一个前提要求：数据必须是平稳的，即数据的平均变化是恒定的。非平稳数据需要经过差分处理，转变为平稳数据。因此在使用ARIMA模型之前，需使用Augmented Dickey-Fuller Test(ADF)对数据进行平稳性预测. ARIMA 模型的参数包括三个部分：p、d、q，分别表示自回归阶数、差分阶数和移动平均阶数。这些参数共同构成了 ARIMA 模型的复杂性。为了确定 ARIMA 模型的合适参数，通常使用自相关图 (ACF) 和偏自相关图 (PACF) 进行分析。ACF 图描述相邻时间序列数据之间的相关性，帮助确定移动平均阶数 q。而 PACF 图衡量在考虑单独时间滞后影响时的数据水平，帮助确定自回归阶数 p[^90]。此外，参数 d 的值是通过差分处理的次数来确定的，差分处理可以将数据转化为平稳序列。

   为了选择最佳的 ARIMA 模型，通常会使用赤池信息准则 (AIC) 来评估不同模型的性能，公式见下，AIC 越小表示模型越好[^89]。通过比较不同参数组合的 AIC 值，可以选择出最合适的 ARIMA 模型，从而实现时间序列数据的准确预测。
$$
   AIC= 2k-2l
$$
   where $l$ is a log-likelihood, and $k$ is a number of parameters.

   总结来说，ARIMA 模型是一种适用于时间序列数据回归预测的方法，其中包括了自回归、差分和移动平均等步骤来构建模型。通过分析 ACF 和 PACF 图以及使用 AIC 选择最佳模型，可以准确地预测时间序列数据的未来趋势。在这个项目中，ARIMA模型主要作为预测实际价格的基线模型。

   

2. RandomForest Classifier: 对于价格方向预测的二分类问题，在这个项目中，使用随机森林分类器作为基线模型, which is一种在分类领域常用的模型。随机森林是一种机器学习方法，通过构建多个决策树来进行操作。最终的决策是基于大多数树的输出，由随机森林进行选择。使用随机森林算法的主要优点是降低过度拟合的风险和减少所需的训练时间。它提供高准确性，并且在大型数据集上可以高效运行，同时适用于分类和回归问题[^87]。决策树是随机森林算法的基本构建块，可应用于各种机器学习应用。然而，为了学习高度不规则模式而生长得很深的树往往会导致过度拟合训练集。数据中的轻微噪音可能会导致树以完全不同的方式生长。这是因为决策树具有非常低的偏差和高方差。随机森林通过在特征空间的不同子空间上训练多个决策树来解决这个问题，但代价是稍微增加偏差。这意味着森林中的树木都看不到完整的训练数据，而是递归地将数据分割成多个分区。在特定节点上，通过询问属性问题来进行分割，分裂标准的选择基于一些杂质度量，如香农熵或基尼杂质。随机森林是一种用于分类、回归和其他任务的集成学习方法，通过在训练时构建大量决策树并输出各个单独树的类别或均值预测来进行操作。随机决策森林纠正了决策树过度拟合训练集的倾向。在分类器应用过程中，使用网格搜索和5折交叉验证来调整随机森林模型的超参数，通过搜索不同的`n_estimators`和`max_depth`值的组合。最佳的超参数组合是基于交叉验证准确性确定的，并且最终的模型将在整个训练数据集上进行拟合。随机森林缺点是可能在具有明显的顺序或时间序列的数据类型上表现不佳，因为它们未能有效捕获这些依赖关系。

#### Deep learning-based Models

1. CNN

   卷积神经网络（CNN）是一种专门用于处理结构化网格数据（如图像和序列）的神经网络。它利用卷积层从输入数据中自动学习空间层次的特征。典型结构包括卷积层、池化层和全连接层[^86]。CNN能自动从输入数据中学习有关的层次化特征，适用于图像和序列处理任务，其输入一般为二维矩阵。卷积层和池化层通常被组合n次以提取一维的高级特征向量。这些特征向量由隐藏层和输出层处理[^85]。 CNN 基本架构的示例如图所示，卷积层由可学习的过滤器组成，这些过滤器在输入数据上滑动(卷积)以检测各种特征。

   ![image-20230824214039804](https://raw.githubusercontent.com/RooNat/Myimages/main/2023/08/upgit_20230824_1692915343.png)

2. LSTM

   长短时记忆（LSTM）神经网络，是循环神经网络（RNN）架构的一种扩展[^85]，旨在保留时间信息的同时还能在其单元内实现长期记忆。与传统的带有反馈循环的RNN不同，LSTM神经网络由记忆块或单元组成。作为循环神经网络层类别的一部分，LSTM层通过个别的记忆单元和自适应门单元（输入、遗忘和输出）增强信息流控制。每个LSTM记忆单元由三个关键组件支持：输入门、遗忘门和输出门。遗忘门使用先前状态的输出或短期状态从存储块（长期状态）的先前状态中检索相关信息，控制要丢弃哪些信息。输入门确定生成当前状态所需的信息，控制要添加到单元或长期状态的信息[^85]。最后，输出门充当过滤器，控制当前状态中哪些信息对输出（预测）或短期状态产生影响。LSTM块的架构如图所示。

   LSTM层的一个重要优点是其能力，能够识别时间序列数据中的短期和长期相关性，有效解决了梯度消失问题。其在有效分析与时间相关的变量方面表现出色，适用于分类和回归任务。在本研究中，LSTM模型被用于与价格预测相关的回归和分类任务。

   

3. CNN-LSTM

   除CNN模型和LSTM模型外，本项目尝试使用CNN-LSTM混合模型。Saúl等人曾针对价格预测问题提出过一种CNN-LSTM模型[^30]，该模型含有5个卷积层模块和1个LSTM模块，最后经过8层全连接层并进行输出。CNN-LSTM混合模型的优点是使模型能够从空间和时间维度提取层次化特征，提升处理复杂模式的性能，并能缓解过拟合，但是提高了模型的复杂度和处理时间，且对数据量具有要求。其基本结构如图所示。本研究基于此模型对数据集进行测试，并与CNN和LSTM模型的性能进行比较。

   





---

### BackTesting Strategy

action recommendation 旨在根据训练好的价格走势预测模型对下一时刻的买卖策略进行推荐，下一时刻可进行的操作有三种，即“买入”，“卖出”，“等待”。

回测策略是基于历史价格数据的迭代过程，用于根据预训练的LSTM模型的预测结果和设定的阈值来做出买入或卖出的决策。以下是策略的工作步骤：

1. 首先，将curr_holding初始化为False，表示初始时不持有任何仓位。
2. 然后，通过历史价格数据进行迭代，逐个时间步地检查模型对下一个时间步的价格方向预测。
3. 如果当前未持仓且模型预测的价格上升的概率超过设定的阈值，策略会触发买入操作，购买加密货币。
4. 如果当前已持仓且模型预测的价格方向概率下降到一定的阈值以下，策略会触发卖出操作，出售加密货币。
5. 如果模型预测的价格方向概率落在设定的阈值之间，策略会等待（标记为'w'）。
6. 每次买入、卖出或等待操作，策略会记录事件并将其存储在events_list中。
7. 如果选择启用图表可视化（plot=True），策略将生成一个图表，用于展示价格数据和交易事件的变化。图表中将显示买入、卖出点以及等待期。
8. 图表还会呈现投资回报率（ROI）和投资组合价值随时间的变化情况。

总而言之，该策略利用经过训练的LSTM模型对加密货币价格的预测，根据模型预测和预设的阈值来执行买入和卖出的操作。通过图表可视化交易事件和投资绩效，展示了投资组合在时间上的表现变化。这一方法更加正式地描述了回测策略的执行过程和结果呈现。



> To begin with, the flag of trading action is set to False, indicating that no positions are currently held. The historical price data is then iterated through, one time-step at a time, checking the model's predicted price direction for the next time-step. If no positions are currently held and the probability of the model's predicted price direction going up exceeds a predetermined threshold, the strategy triggers a buy operation to purchase the cryptocurrency. Conversely, if a position is currently held and the probability of the predicted price direction going up falls below a certain threshold, the strategy triggers a sell operation to sell the cryptocurrency. If the probability of the model's predicted price direction falls between the set thresholds, the strategy waits (marked with a 'w'). For each buy, sell, or wait operation, the strategy records the event and stores it in the "events_list."  If chart visualisation, the strategy will generate a chart that shows changes in price data and trading events. The chart will display buy and sell points, as well as waiting periods. Additionally, the chart will present the return on investment (ROI) and portfolio value over time.  
>
> In summary, the strategy utilizes a trained model for cryptocurrency price prediction to execute buy and sell operations based on model predictions and preset thresholds. Trading events and investment performance are visually represented through charts, which illustrate how the portfolio's performance has changed over time. This approach provides a more formal description of the execution process and presentation of results for the backtesting strategy.

## Results and Discussions

本章将针对第三章提出的方法进行实施并对结果进行讨论和评估，涉及到在数据的准备阶段中数据的选择以及公众情绪与比特币价格之间的相关性分析。最终将针对项目的三个阶段的研究结果进行模型的性能评估和讨论，其中包括第一阶段的比特币价格回归预测，第二阶段的比特币价格分类预测，以及第三阶段的行为推荐策略的回测。

> This chapter will implement, discuss, and evaluate the results of the methodology presented in Chapter 3, which involves data selection during the data preparation phase and the analysis of the correlation between public sentiment and Bitcoin price. The chapter will conclude with the evaluation and discussion of the model's performance in relation to the outcomes from the three project phases: the regression prediction of Bitcoin price in the first phase, the classification prediction of Bitcoin price in the second phase, and the backtesting of the behavioral recommendation strategy in the third phase.

### Data Preparation and Correlation Analysis

#### Google Trends

在第三章中，介绍了两种Google Trends数值校正方法。第一种方法涉及将校正数值替代原始数值，并在下一周的比率计算中使用。第二种方法则是在不替代原始数值的情况下进行校正。在先前的相关研究中，主要采用了第一种方法。这种方法可能使时间序列更加平滑，同时能确保校正按顺序进行，以保持时间关系。但是，也有一小部分研究采用了第二种方法，因为它保持了原始值的完整性。

在数据准备阶段，分别应用了这两种方法，并利用斯皮尔曼方法计算了调整后的Google Trends数据与收盘价之间的相关性。优先选择了与收盘价相关性较高的数据进行进一步研究。

> Chapter 3 discusses two approaches for correcting Google Trends values. The first method involves substituting corrected values for the original ones and using them in the next week's rate calculation. On the other hand, the second method corrects the values without replacing the original ones. Previous studies have mainly employed the first method, which may result in a smoother time series while maintaining temporal relationships. However, a few studies have also used the second method to preserve the original values' integrity. During the data preparation stage, both methods were separately applied, and the Spearman method was used to calculate the correlation between the adjusted Google Trends data and the closing price. High correlation data with closing prices were prioritized for further research.

首先，在六个月的数据范围内检验了相关性。使用替代原始数值的方法显示出收盘价与谷歌趋势之间的直接相关性更强，为-0.26，表明了它们之间的直接负相关性。相反，在整个六个月的数据范围内，使用第二种方法验证了微弱的整体正相关性，为0.17。

为了进一步验证，从一个月的数据开始，逐步增加了数据范围。结果显示，在前五个月，数据时间范围越短，比特币价格与谷歌趋势数据之间的直接负相关性越强，如图1所示。此外，存在使用替代值的谷歌趋势调整数据的相关性绝对值始终大于不使用替代值的情况。因此，选择继续使用第一种方法得到的谷歌趋势数据进行后续研究。图2展示了原始趋势数据与调整后的谷歌趋势数据的对比，表明调整后的方法能够保持时间上的连续性。

图1为两种数据分别与比特币Close price的相关性，图2为原始谷歌趋势数据与调整后的谷歌趋势数据值的可视化。



在获得谷歌趋势研究数据后，使用Granger Causality testing对比特币回报和价格变化在48小时内对谷歌趋势数据变化的滞后影响进行了测试。数据范围从一个月开始，逐月增加，直至六个月，并进行了分别的测试。结果显示，比特币的价格回报和变化对谷歌趋势数据的变化在六个月内都呈现出显著的因果关系。具体而言，从第一个滞后期开始，统计检验的p值均小于0.05，表明价格回报和价格变化能够影响谷歌趋势的变化。

随后，进行了反向测试。结果显示，在使用一个月至五个月的数据范围内，谷歌趋势数据从第10个滞后期开始与价格回报存在因果关系。而当数据范围扩展至六个月时，谷歌趋势数据从第二个滞后期开始就对比特币价格回报产生预测能力。这表明，在不同的时间范围内，谷歌趋势对比特币收盘价以及比特币价格回报的滞后影响可能会有所变化。然而，总体而言，比特币收盘价与谷歌趋势之间存在稳定的Granger因果关系。

#### Tweets Data

推文数据包含情感分析推文的情感得分，包括来自Vader和Textblob情感分析的compound score、polarity score和subjectivity score。基于Loughran和McDonald金融语料库的情感得分特征被删除，因为分析结果得到大量的0值，无法获得有效的情感倾向结果。另外，还需考虑情感分数中存在许多0值（中性值）的问题。研究最初假设这些可能是机器人发布的无效信息，因此可以采取两种处理方法：一种是去除中性的compound score并按小时取平均值，另一种是不去除中性值直接按小时取平均值。本研究分别尝试了这两种方法，并使用相关性测试进行了验证。首先，对两种数据进行了斯皮尔曼相关性测试，去除了相关性值几乎为0的subjectivity score。随后，对compound score和polarity score进行了进一步的直接相关性分析。与谷歌趋势的分析类似，本研究对连续的1到6个月的数据进行了相关性分析。结果显示，直接相关性在单个月份上最高，可达0.51，如图3所示。如图4所示，推文情感值与比特币价格具有一定程度上的相似趋势。在前1到5个月，比特币收盘价与推文compound score的相关系数逐渐减小，当使用六个月的数据时，相关性明显回升。因此，提出了谷歌趋势与推文情感之间可能存在相关性的假设，并通过测试在六个月的数据上验证了这一假设，两者的相关性为-0.45，呈现出明显的负相关性。

另一方面，在使用三个月份的数据时，出现了异常值的情况，可能是因为该月份的情感中性得分过多。尽管如此，去除中性值的情感平均值与比特币价格的整体相关性低于不去除中性值的情感平均值，这表明保留中性情感值是有必要的。因此，本研究最终决定使用不去除中性值的情感平均值作为特征之一。

需要注意的是，在一个月内，推文数量与比特币价格呈现负相关，但数据增加到三个月到四个月份时，线性相关性不再显著。然而，这并不代表推文数量与比特币价格回报之间不存在因果关系。为了验证这一点，使用了Granger因果关系测试。有趣的是，尽管在一个月份具有最强的相关性，但是推文数量与价格回报之间不具有双向因果关系。相反，在连续的2到6个月份数据上，推文数量对价格回报具有1 lag或11到28个lag的滞后影响。

同样，对于推文情感，尽管两者具有强直接相关性，且价格回报对推文情感具有强预测能力，但推文情感对价格回报并没有因果关系，仅在短期内对价格移动方向具有2到19个小时的滞后影响。

综上所述，推文情感与比特币价格具有相关性，但推文情感对比特币价格回报没有预测能力。推文数量与比特币价格之间没有强线性相关性，但在一定时期内，其对比特币价格回报具有预测能力。

> The tweet data encompasses sentiment scores derived from analyzed tweets, which include the compound score, polarity score, and subjectivity score obtained through the Vader and Textblob sentiment analyses. Sentiment score features based on the Loughran and McDonald financial corpus were excluded due to a substantial number of resulting 0-values, rendering meaningful sentiment trends unattainable. Moreover, the prevalence of numerous 0 values (neutral values) within the sentiment score warrants consideration.
>
> Initially, it was hypothesized that these neutral values might be attributed to robot-generated information. Thus, two approaches were explored to address this issue: the first involved excluding the neutral composite score and averaging hourly, while the second involved a direct hourly average without removing the neutral values. Both methods were separately attempted in this study and validated via correlation testing. To begin, a Spearman correlation test was conducted on both datasets to eliminate the subjectivity score, which exhibited an almost negligible correlation. Subsequently, direct correlation analyses were extended to the compound score and polarity score. Analogous to the Google Trends analysis, this study performed correlation analyses on consecutive data spanning 1 to 6 months. The findings highlight that the highest direct correlation is observed within a single month, reaching up to 0.51, as depicted in Figure 4, where tweet sentiment values and Bitcoin price demonstrate a somewhat analogous trend. The correlation coefficient between Bitcoin's closing price and the tweet compound score dwindles during the initial one to five months, but notably increases when six months of data are included. Hence, a hypothesis suggesting a conceivable correlation between Google Trends and tweet sentiment was proposed and validated over a six-month dataset, revealing a significant negative correlation of -0.45.
>
> Conversely, when using a three-month data window, outliers emerge, potentially stemming from an excessive number of sentiment-neutral scores for that period. Nonetheless, the overall correlation between the sentiment mean (neutral values excluded) and Bitcoin's price remains lower than the sentiment mean with neutral values retained, implying the necessity of preserving neutral sentiment values. Consequently, this study decided to employ the sentiment mean without eliminating neutral values as a feature.
>
> It is noteworthy that, within a one-month timeframe, the number of tweets exhibits a negative correlation with Bitcoin's price. However, as the data expands to three to four months, the linear correlation loses significance. Nevertheless, this absence of linear correlation does not preclude the existence of a causal relationship between tweet counts and Bitcoin price returns. To validate this, a Granger causality test was applied. Intriguingly, despite the strongest correlation occurring within a single month, there is no bidirectional causality between tweet counts and price returns. Instead, tweet counts exert a lagged effect, whether a 1-lag or spanning 11 to 28 lags, on price returns over consecutive months ranging from 2 to 6 months.
>
> Similarly, in the context of tweet sentiment, despite the robust direct correlation and the potent predictive influence of price returns on tweet sentiment, tweet sentiment itself does not exhibit a causal effect on price returns. Rather, it manifests a lagged effect on the direction of price movement, spanning 2 to 19 lags within the short term.
>
> In summary, there exists a correlation between tweet sentiment and Bitcoin price, but tweet sentiment lacks the predictive capability for Bitcoin price returns. While a strong linear correlation between tweet counts and Bitcoin price is absent, tweet counts hold predictive power for Bitcoin price returns over a defined period.



#### Reddit Posts


第三章中引入了一种方法，借鉴了先前关于推文情绪得分的调整方法，将每条帖子的点赞比率作为权重来调整情绪得分，旨在取得更好的结果。然而，通过对无权重情绪得分平均值和有权重情绪得分平均值与比特币价格之间的斯皮尔曼相关性进行分析（如图所示），结果显示两种数据得到的相关性得分相差不大。进一步进行格兰杰因果关系测试后，发现在各个时间范围内，三种情绪得分与比特币价格回报之间都不存在双向因果关系。

然而，对于比特币价格移动方向的预测，在三个月以上的时间范围内，所有情绪得分都表现出3个滞后及以上的预测能力，且滞后数相同。因此，可以得出结论，是否采用点赞比率作为权重对结果没有影响。此外，Reddit帖子与每小时比特币价格的线性相关性并不强烈，而Reddit帖子对比特币价格变化的影响更多地体现在长期影响方面。

综上，对比特币价格回报和价格移动方向具有预测能力的features和相应的滞后数如Table所示。由于在三种不同的特征上进行相关性判断和因果测试时，在一个月的短期数据和六个月的长期数据显示了突出的特点，即短期数据相关性较强，长期数据相关性回升。因此本研究将重点关注这两个时间范围的结果。在特征选择过程中，对于价格预测问题，由于谷歌趋势、推文情绪分别在每小时间隔的数据有较强的相关性，因此其可以作为一定的预测指标。本文重点关注其对比特币价格回报和移动方向的影响，结果显示，对于价格回报问题，谷歌趋势在六个月的长期数据上，可以选用2个以上的滞后作为特征之一，除此之外，1, 30到32个滞后的推文数量对价格回报可能有预测能力，因此选做特征之一。对于价格移动方向预测，在六个月的长期数据上，谷歌趋势对其在10个滞后以上产生影响，推文polarity score以及Reddit posts的相关情绪得分均可对其有一定的滞后影响，因此也作为特征。

### Data Pre-processing

在选择好特征后，在将数据输入到模型之前，需要对时间序列数据进行重构，对于每个待预测的值，其过去的一段时间时间点的特征和目标值都将被作为输入特征，本小节将描述实验过程中数据的重要预处理步骤，包括数据集的重构，数据集的分割，以及数据的归一化。

> After feature selection, the time series data requires reconstruction before being fed into the model. For each value to be predicted, the features and corresponding target values at a past time point are utilized as input features. This subsection outlines crucial data preprocessing steps undertaken during the experiment, encompassing the reconstruction of dataset, dataset splitting, and data normalization.

#### Dataset for time series analysis

时间序列预测是利用历史值和相关模式来预测未来活动的分析技术。如果预测下一个时间点的收盘价或其他价格模式，则需要利用一定的时间步长的历史数据来对下一刻的值进行预测。例如，当只使用过去15个时间点的价格数据预测下一时刻的价格数据时，特征值和目标值将分别为$[X_1,X_2,\cdots, X_{15}]$, 和$[X_{16}]$ 。其中$X$为价格。如图所示

> Time series forecasting is an analytical technique that leverages historical values and associated patterns to predict future trends. It involves predicting the closing price or other price patterns at the next time point based on historical data from a specific time step. For instance, to predict the next time point's price using price data from the past 15 time points, the input values and target value would be $[P_1, P_2, \cdots, P_{15}]$ and $[P_{16}]$ respectively, where $P$ represents the price.

（滞后图）

创建具有**滞后值或延迟值**的时间序列数据集涉及将数据安排在每个观察中，不仅与其自身特征相关，还与之前时间步长的过去观察相关联。这种滞后在捕捉跨时间的依赖性和关系方面非常有用。这项研究的问题之一就是考虑较佳滞后，从而发现比特币相关的社交媒体数据对比特币价格的影响。通过前文所述的Granger Casuality Testing方法和斯皮卡曼相关性可以在一定程度上确定公众情绪的滞后值，利用比特币相关公公情绪得分和比特币价格数据创建具有滞后特征的时间序列数据集[^79]，如图所示，其是通过将各个特征向后移动所测试滞后的相应时间点来创建的。假设时间步长为7，重新创建的滞后数据集的每个时间点的特征和目标值将如图所示。

> Constructing a time series dataset with a lag or delay value involves organizing the data in a manner that associates each observation not only with its own attributes but also with past observations from previous time steps. These lags are instrumental in capturing temporal dependencies and relationships. A challenge in this study is to determine the optimal lag duration to effectively unveil the influence of Bitcoin-related social media data on Bitcoin's price. The lagged values of public sentiment can be somewhat determined through methods like Granger Causality Testing and Spearman correlation, as discussed earlier. By employing Bitcoin-related public sentiment scores and Bitcoin price data, a time-series dataset was formed with lagged features[^79]. This entailed shifting individual features backward by the corresponding lag time being examined, as illustrated in Figure points. Assuming a time step of 7, the features and target values for each time point in the restructured lagged dataset would resemble those shown in Fig.

（重塑图）

#### Data Splitting

在进行数据处理后，本研究一共有4344条数据，横跨六个月，为了训练和测试模型的准确性，所有模型都使用80:20的训练-测试比率分割数据集。这是一种常见的分割比率，可以使得模型有很大的比例的数据来进行训练，也有合适数量的数据进行测试。为了在训练过程中评估模型性能并进行模型优化，还需设置验证集比例为0.2，从而从训练集中分割部分验证集进行参数调整。为测试不同的模型和参数，每组参数都在3个不同的打乱的数据集上进行测试，以防数据的分割带来的偏差[^79]。训练集和测试集的分割需要在归一化之前进行，避免测试集的信息泄漏，将降低模型评估的准确性和真实性。

> After data preprocessing, a total of 4,344 data points spanning six months were available for this study. To ensure accurate training and testing of the models, the dataset was divided using a common 80:20 training-testing ratio. This ratio allows for a substantial amount of data for model training while maintaining an adequate portion for testing. To effectively assess model performance and conduct optimization during training, a validation set ratio of 0.2 was also established. This involved extracting a portion of the validation set from the training data for parameter tuning. To prevent bias stemming from data segmentation, different sets of parameters were evaluated on three distinct shuffled datasets, ensuring robustness[^79]. It's important to note that the division of training and test sets must be performed before normalization to prevent information leakage from the test set, which could compromise the accuracy and realism of model evaluation.

#### Data Normalization

初始的数据集中各个特征的范围不一致，会加剧模型训练的难度，因此在训练之前需对数据进行归一化，归一化是数据预处理中的重要步骤，为了保证机器学习算法的预测准确可靠性。通过将数据缩放到标准范围，归一化确保所有特征对模型的预测具有相同的影响力。同时，它减少了模型对异常值和极端数值的敏感性。使用MinMaxScaler[^81]分别对训练机和测试集进行归一化处理，是为了确保机器学习模型中的所有特征都有一致的尺度。通过将数据转换为0到1之间的统一范围，MinMaxScaler可以减少由于价格大小差异引起的错误[^80]。 

(看情况是否添加公式)

### Phase 1: Hourly Price Pridiction

#### Performance Evaluation Methods

对于价格和其回报的预测问题，选用RMSE和R2 Score两个基本回归问题评估指标进行模型性能评估。均方根误差（RMSE）是一种用于评估预测模型性能的指标，它衡量了预测值与实际观测值之间的平均差异程度。RMSE越小，表示模型的预测结果与数据集中的真实观测值之间的差异越小，说明模型对数据集的拟合效果越好[^75]。RMSE的数学公式如下：

> For evaluating the prediction of prices and their returns, two fundamental regression assessment metrics, namely RMSE and R2 Score, were selected to gauge model performance. The Root Mean Square Error (RMSE) is a metric utilized to evaluate forecasting models, quantifying the average level of disparity between predicted values and actual observations. A lower RMSE indicates closer alignment between the model's predictions and the real dataset, signifying better model fit[^75]. The mathematical formula for RMSE is as follows:

$$
RMSE=\sqrt{\dfrac{1}{N} \sum_{i=1}^{N}(y_i-f(\vec x_i))^2}
$$

其中$N$为总共的数据点的数量。$y_i$是第 i 个实际值， $f(\vec{x_i})$是其相应的预测值。

R2（R-squared）是另一种用于评估预测模型的指标，它表示回归模型中因变量的方差中有多少可以被自变量解释的部分。R2的取值范围在0到1之间，数值越接近1，说明模型能够更好地解释因变量的变异性，从而更好地拟合数据集[^75]。R2-Score can be calculated as:

> R2 (also known as R-squared) is a metric used to assess predictive models. It measures the extent to which the independent variables in a regression model can explain the variance of the dependent variable. The R2 score ranges from 0 to 1, with values closer to 1 indicating a better fit between the model and the dataset. 

$$
R^2=1-\dfrac{\sum_{i=1}^{N}(y_i-f(\vec x_i))^2}{\sum_{i=1}^{N}(y_i-\bar y)^2}
$$

where $\bar y$ is the mean of the target values $y_i$

除此以外，对于时间序列模型的评估，还可以使用MAPE对模型的准确性进行测试，MAPE是实际数据与预测数据之间总体误差百分比(差异)的平均值。低MAPE值表示结果值接近其实际值[^74]。这里是MAPE的方程：

> Furthermore, when assessing time series models, their accuracy can be measured using MAPE. This stands for Mean Absolute Percentage Error and is calculated as the average percentage difference between the predicted and actual data. A lower MAPE value suggests that the predicted value is more accurate to the actual value[^74]. The formula for MAPE is as follows:

$$
MAPE = \dfrac{1}{N}\sum_{i=1}^{N}\dfrac{|y_i-f(\vec{x_i})|}{|y_i|}
$$

#### Model Selection

##### ARIMA

项目初期，使用ARIMA作为基线模型对收盘价进行预测，由于收盘价不是平稳的数据，将其通过一级差分计算价格回报转换为平稳的数据，并利用ACF和PACF图分别确定ARIMA模型中的q值和p值，其表示当前观察值与其过去滞后值的相关性。在只有一个月的短期数据上，其p值与q值都有11和24两种选择，利用AIC对模型进行选择，最终确定最佳模型为ARIMA(11,1,24)，其具有最低AIC值。利用相同方法对探求六个月数据的p值和q值，其p值和q值也都有10和15两个滞后选择，对不同滞后组合进行模型训练并得到ARIMA(10,1,10)具有最低AIC值，为最优模型。利用这两种模型作为基线模型且仅使用历史价格作为特征分别对两个时间范围的收盘价进行预测。

> At the project's outset, ARIMA served as the baseline model for predicting closing prices. Given the non-smooth nature of closing prices, a first-order difference was applied to achieve smoother data. ACF and PACF plots aided in determining the q-value and p-value in the ARIMA model, reflecting the correlation between current observations and their lagged past values. For the one-month short-term data, there existed two options for p and q values: 11 and 24. AIC was utilized to select the optimal model, resulting in ARIMA(11,1,24) due to its lowest AIC value. Employing a similar approach with six-month data, p and q values of 10 and 15 were considered. Through model training across varying lag combinations, the optimal model emerged as ARIMA(10,1,10). Leveraging these two baseline models, solely historical prices were employed as features to predict closing prices for both time horizons.

##### LSTM

LSTM在价格回归预测问题上，不要求数据具有平稳性，因此项目初期直接使用LSTM对收盘价进行预测，仍使用在ACF和PACF获得p值和q值来确定timestep，一个月的短期数据设置timestep为11或者24，六个月的长期数据设置timestep为10或者15，直接使用具有32个神经元，batch size为32的单层LSTM模型分别对两个数据集进行5折交叉验证并选用最佳timestep。最终得到短期的数据在timestep为24时可获得最低平均RMSE，为376.56，其MAPE为0.06，较长期的数据在timestep为10时可获得最低平均RMSE，为2724.83，其MAPE为0.104。其与ARIMA基线模型性能比较结果如Table和图5所示，ARIMA模型的预测结果成一条直线，但是LSTM可以对价格进行拟合，这可能是因为ARIMA是线性模型，很难捕捉复杂特征，但是LSTM为非线性模型，可以自动学习输入数据的时序关联性和非线性特征。此步可以初步判断，在相同的timestep下，在两个时间段的数据集上，LSTM比ARIMA模型的表现更好，此时完成初步模型选择，并使用LSTM模型进行接下来的Price returns预测，并验证社交媒体情绪与Google Trends数据对returns的预测能力。

==Table and 图==

> LSTM在价格回归预测问题上不要求数据具有平稳性。项目初期直接使用LSTM对收盘价进行预测，仍利用ACF和PACF获得p值和q值来确定时间步（timestep）。对于一个月的短期数据，将timestep设为11或24；对于六个月的长期数据，将timestep设为10或15。单层LSTM模型具有32个神经元，batch size为32，在两个数据集上分别进行5折交叉验证并选择最佳的timestep。结果表明，短期数据在timestep为24时获得最低平均RMSE为376.56，MAPE为0.06。相比之下，长期数据在timestep为10时获得最低平均RMSE为2724.83，MAPE为0.104。其与ARIMA基线模型性能比较结果如Table和图5所示。
>
> 与基线模型ARIMA相比，LSTM能够更好地拟合价格预测。ARIMA的预测结果呈现一条直线，可能因为其为线性模型，难以捕捉复杂特征。相反，LSTM作为非线性模型，能够自动学习时序关联性和非线性特征。在相同的timestep下，LSTM在两个时间段的数据集上的表现明显优于ARIMA模型。这一步的初步模型选择完成后，将使用LSTM模型进行接下来的价格回报预测，并评估社交媒体情绪和Google Trends数据对价格回报预测的能力。
>
> LSTM does not require data smoothing for price regression prediction. In the project's early stage, LSTM was directly employed for closing price prediction. ACF and PACF were used to determine p-values and q-values for the time step (timestep) selection. For short-term one-month data, timestep was set to 11 or 24, while for long-term six-month data, it was set to 10 or 15. A single-layer LSTM model with 32 neurons and a batch size of 32 was utilized for 5-fold cross-validation on both datasets to identify the optimal timestep. The results revealed that the short-term data achieved the lowest average RMSE of 376.56 and MAPE of 0.06 with a timestep of 24, while the long-term data achieved the lowest average RMSE of 2724.83 and MAPE of 0.104 at a timestep of 10. Performance comparisons between LSTM and the baseline ARIMA model are presented in Table and Figure 5.
>
> LSTM outperforms the baseline ARIMA model in fitting price predictions. ARIMA's prediction outcome presents a linear trend, likely due to its linear nature, which struggles to capture complex features. Conversely, as a nonlinear model, LSTM automatically captures timestep correlations and nonlinear features. For the same timestep, LSTM consistently outperforms the ARIMA model across both time-series datasets. After this preliminary model selection, the LSTM model will be deployed for subsequent price return predictions and assessing the predictive capacity of social media sentiment and Google Trends data.

进行Price的returns预测时，对LSTM模型的神经元数，batch size，layer数，激活函数以及优化器进行5折交叉验证，以选择最佳超参数。其中，神经元数在32，64，128中选择，batch size在30, 64, 80中选择，layer数在1，2，3中选择，激活函数在'relu','linear'之间进行选择，优化器在'adam'和'rmsprop'中进行选择，根据平均最低MAPE和最高$R^2$值选择最佳LSTM模型，最终选择具有32个神经元，batch size为80，激活函数为'relu',且优化器为'adam'的单层LSTM模型。使用EarlyStopping回调来在验证损失不再改善时停止训练，以防止过拟合。

> 在进行价格回报预测时，通过5折交叉验证来选择LSTM模型的超参数，包括神经元数、批量大小、层数、激活函数和优化器。神经元数的选择范围为32、64、128，批量大小的选择范围为30、64、80，层数的选择范围为1、2、3，激活函数的选择范围为'relu'和'linear'，优化器的选择范围为'adam'和'rmsprop'。通过对平均最低MAPE和最高$R^2$值进行评估，选择出最佳的LSTM模型。最终确定采用具有32个神经元、批量大小为80、激活函数为'relu'且优化器为'adam'的单层LSTM模型。此外，采用EarlyStopping回调来在验证损失不再改善时停止训练，以防止过拟合。

#### Results and Discussion

使用所选择的最佳LSTM模型及之前章节中选择的特征分别对1个月的短期数据和6个月的长期数据进行价格回报预测，每种特征组合运行5次，通过平均$R^2$ score对结果进行评估，结果可以在Table中看到，起先只使用历史的价格回报作为特征输入模型，并且根据格兰因果关系测试结果逐渐增加可能具有影响的特征。事实上，由于比特币价格震荡较大且回报值普遍较低，仅使用LSTM和价格回报值还不足以有较好的预测结果，但是仍可以观察到，通过增加具有滞后值的特征，可以一定程度上提高$R^2$ score，在短期数据上，测试对原数据分别添加10个滞后，15个滞后和25个滞后的谷歌趋势特征，最终得到15个滞后值时能得到最好的表现。而长期数据上，分别在原特征上添加具有2个滞后的谷歌趋势特征和具有1个滞后的推文数量，结果显示谷歌趋势滞后数据可以极大程度的提高价格回报预测能力，这一点在具有其他滞后值的谷歌趋势特征上也得到了验证。但是推文数量对价格回报的预测能力的体现不显著，反而起反作用，在推文数量与谷歌趋势的特征组合中，只有在30个滞后的推文数量与2个滞后的谷歌趋势特征进行组合时，能够稍微提高预测能力，却反而低于仅使用谷歌趋势特征值的预测效果，这可能是因为推文数量特征的添加，导致了预测效果减弱。

**==Table==**

因此可以得到结论，谷歌趋势数据对价格回报确实存在一定的预测能力，且从长期影响看，谷歌趋势对价格回报产生滞后影响的间隔较短，平均在2个小时之后即可对价格回报产生影响。但是推文数量对价格回报的影响无论在短期还是长期都不明显。

>
> 采用最佳LSTM模型及前文选择的特征，分别对1个月短期数据和6个月长期数据进行了价格回报预测。每种特征组合进行了5次运行，并通过平均$R^2$分数进行了评估，具体结果见表格。初始阶段，仅使用历史价格回报作为特征输入模型，并根据格兰因果关系测试逐步引入可能具有影响的特征。考虑到比特币价格的大幅波动和回报值普遍较低，单纯使用LSTM和价格回报值并不能产生良好的预测效果。然而，值得注意的是，通过引入具有滞后值的特征，可以在一定程度上提高$R^2$分数。
>
> 在短期数据方面，通过对原始数据分别添加10个、15个和25个滞后的谷歌趋势特征进行测试，结果表明在15个滞后值时表现最佳。对于长期数据，通过添加具有2个滞后的谷歌趋势特征以及具有1个滞后的推文数量特征，得出谷歌趋势滞后数据极大地提升了价格回报预测能力的结论。这个发现在其他滞后值的谷歌趋势特征中得到了验证。然而，推文数量对价格回报的预测能力影响不显著，甚至可能产生反效果。在推文数量与谷歌趋势特征组合中，仅有在30个滞后的推文数量与2个滞后的谷歌趋势特征相结合时，预测能力稍有提升，但仍低于仅使用谷歌趋势特征值的预测效果。这可能是由于引入推文数量特征导致预测效果减弱。
>
> 综上所述，谷歌趋势数据在价格回报预测方面确实具有一定的能力。从长期影响来看，谷歌趋势对价格回报的滞后影响较短，通常在约2小时后即可产生影响。然而，无论在短期还是长期内，推文数量对价格回报的影响都不明显

### Phase 2: Hourly Price Movement Prediction

#### Performance Evaluation Methods

在预测价格移动方向时，需要使用二分类模型，评估分类模型的准确性时，本项目使用了Accuracy和F1-Score两个模型评估标准。这两个指标都在true positives, false positives, true negatives, and false negatives[^76]的基础上创建，如表所示，其中$Down$表示价格方向下降，$Up$表示价格方向上升。

> When predicting price movement direction, a binary classification model is essential. To gauge the accuracy of this classification model, two evaluation metrics were employed in the project: Accuracy and F1-Score. These metrics were derived from true positives, false positives, true negatives, and false negatives[^76], as illustrated in the table. In this context, the label $Down$ signifies a decrease in price direction, while $Up$ signifies an increase in price direction.

| True\Predicted | Down | Up   |
| -------------- | ---- | ---- |
| Down           | TP   | FN   |
| Up             | FP   | TN   |
|                |      |      |



F1 分数是评估分类器性能的常用指标，尤其是在处理不平衡类或假阳性和假阴性具有不同影响的情况时。它是精确度和召回度的调和平均值，其方程如下[^77]：

> When evaluating classifiers, the F1 score is often used as a metric, particularly when dealing with situations where false positives and false negatives have varying impacts or when classes are not balanced. The F1 score is calculated by combining precision and recall using the equation [^77].

$$
\mathrm{F1}=2\cdot \dfrac{\text{Precision} \cdot \text{Recall}}{\text{Precision}+\text{Recall}}
$$

其中Precision和Recall按照方程计算：
$$
\text{Precision} = \dfrac{\text{TP}}{\text{TP}+\text{FP}}
$$

$$
\text{Recall} = \dfrac{\text{TP}}{\text{TP}+\text{FN}}
$$

Accuracy是另一个常用的评估指标[^76]，其计算的方程如下：
$$
\text{Accuracy}=\dfrac{\text{TP+TN}}{\text{TP+TN+FP+FN}}
$$
F1-Score和Accuracy的值都介于0到1之间，值越高代表模型性能越优。

#### Model Selection



使用随机森林分类器作为价格方向预测的基线模型，在六个月的长期数据上进行测验，使用网格搜索和五折交叉验证得到最佳超参数，其中超参数包括决策树的数量（n_estimators）和每棵决策树的最大深度（max_depth）。最终得到最佳决策树最大深度是10，决策树的树的数量是300。将此作为基线模型

###### Deep Learning-based Model

对三个不同的深度学习模型在六个月的长期数据上进行比较，并选择表现最好的模型进行对价格移动方向进行分类预测。同样对CNN模型和LSTM模型进行超参数选择，其中神经元在16，32，64，128中选择，batch size在30，64，80，200之间选择，最终CNN和LSTM都选择为单层，具有32个神经元，batch size为200，CNN和LSTM层的激活函数选择'relu'，全连接层的激活函数选择'linear'。CNN-LSTM混合模型的结构已经在第三章叙述，其中batch size在80时表现更佳。利用CNN, LSTM和CNN-LSTM混合模型对六个月的数据进行初步分类预测，并且只使用历史价格方向和收盘价作为特征，三个模型都在10个步长和15个步长上分别运行五轮去平均值，结果显示LSTM的平均准确率基本高于另外两个模型。以15个步长为例，结果如Table所示，三种模型的预测能力均高于随机森林分类器，CNN-LSTM也可以得到与LSTM相差无几的准确率，但是其在15个步长上分类极其不平衡，这可能是因为CNN-LSTM混合模型结构较复杂，而较复杂的模型需要大量的数据和更多的特征，这也说明了复杂模型不一定会取得更好的分类效果，其还需要大量技术指标。因此，最终仅选择LSTM模型进行进一步的分类预测。

> To select the most effective model for predicting the direction of price movement, three distinct deep learning models were compared using six months of long-term data. Similarly, hyperparameters were fine-tuned for both the CNN and LSTM models, encompassing variations in the number of neurons (16, 32, 64, 128) and batch sizes (30, 64, 80, 200). Ultimately, both the CNN and LSTM architectures were configured as single layers, featuring 32 neurons per layer and a batch size of 200. 'relu' served as the activation function for the CNN and LSTM layers, while the fully-connected layer adopted the 'linear' activation function. The structure of the hybrid CNN-LSTM model was delineated in Chapter 3, revealing optimal performance with a batch size of 80.
>
> Initial classification forecasts were executed through CNN, LSTM, and CNN-LSTM hybrid models, using historical price trends and closing prices as exclusive features over six months of data. Each of the three models underwent five rounds of assessment at 10 and 15 steps, with the averaged outcomes demonstrating the prevalent superiority of the LSTM's accuracy compared to the other two models. For instance, considering 15 steps, the results, as depicted in the table, displayed heightened predictive capability across all three models in contrast to the Random Forest classifier. While CNN-LSTM approached LSTM in terms of accuracy, a conspicuous imbalance was evident in its classification performance over 15 steps. This observation might be attributed to the intricacies of the CNN-LSTM hybrid model's architecture, demanding a more robust data and feature support. This further highlights that complexity in models doesn't necessarily correlate with enhanced classification outcomes; rather, it necessitates substantial underpinning from technical metrics. Consequently, the ultimate decision was to exclusively adopt the LSTM model for subsequent classification predictions.

==Table==

#### Results and Discussion

最终使用LSTM模型对一个月份的短期数据和六个月长期数据进行价格方向预测，与价格回报预测的研究方法相同，通过逐渐增加特征来验证模型的效果。

对于单个月份的短期数据，由格兰杰因果测试得到，只有tweets compound score对其价格方向有一定的滞后影响，因此只对初始特征和额外的滞后的tweets compound score进行预测。初始特征为历史价格和价格移动方向(Label)。添加不同的滞后数的tweets compound score作为特征，并选择最佳滞后数，如Table 所示，10个滞后的tweets compound score将短期内的分类预测效果提升了将近5%。其他滞后的tweets compound score也在不同程度上提升了模型性能。这表现了推文情绪在短期数据上对价格移动方向具有惊人的预测能力，且推文情绪可在2小时到半天内对比特币价格移动方向产生影响。

==Table==

对于六个月份的长期数据，可能对价格方向存在预测能力的特征包含Google Trends，tweets polarity score, 及Reddit posts的相关情绪得分，对这些特征的滞后进行验证，对谷歌趋势进行最佳滞后选择后，得到10个滞后的谷歌趋势模型可产生最佳表现，在一定程度上可以提高模型准确性。将Reddit posts的相关情绪得分看作为一个组合，一同与Google Trends特征以及推文极性情绪进行比较，其结果如Table 所示。结果显示，与原特征相比，添加公众情绪或者谷歌趋势数据均可在一定程度上提高预测能力，其中，在单特征影响，谷歌趋势数据产生的滞后影响更大，可以将平均准确度提高0.3%左右。但是谷歌趋势与推文极性情绪或来源于Reddit的情绪进行组合，还可以进一步提高准确性，其中与推文极性情绪组合得到的准确性最高，将整体准确度提高了0.6%左右。但是当三种不同来源的特征组合在一起时，准确率反而下降。这可能是因为特征值之间存在相互作用。尽管如此，仍可以得出结论，从长期数据来看，谷歌趋势数据对价格方向也可在10小时左右以后产生影响，与价格回报预测得到的结论类似。与谷歌趋势数据进行组合的公众情绪可以得到更高的准确率。三种不同来源的特征均可在不同程度对价格方向产生影响，但是来自Reddit的影响最弱。通过分析，其可能的原因有：Reddit posts的数量较少，与推特这类社交媒体的性质不同，Reddit的posts更多是长文本，并不能像推文一样，用户可以随时发表相关情绪。除此之外，由于数据较少，在处理空缺值时，也可能也会引入噪声。在相关性分析中也可发现，来源于Reddit的公众情绪与比特币价格的关系也相对较弱。

通过特征选择得到的最佳模型的结构可以在图5看到。

> 除此之外，其余公众情绪特征无法对价格方向产生有效预测能力，尤其Reddit posts相关特征，反将模型的准确率降低了2%。因此产生问题，为什么从格兰杰因果测试中得到的滞后特征有的可以有效提高预测能力，而有的反而对模型产生了反效果，降低了预测效果？经过分析得，这可能是因为因果测试得到的公众情绪的滞后值并不连续。无论是价格方向还是价格回报的预测，实验发现，对目标值产生了有效预测能力的特征在因果测试中都产生了连续的滞后值，例如谷歌趋势数据的有效滞后数是连续的10到19（可见表）。而公众情绪得到的大部分滞后都为单个滞后值，仅单个滞后值可能无法提供有效信息。此外，还有可能和模型的复杂度有关，尽管使用了较简单的LSTM模型，其仍然是非线性模型，可能更容易过拟合或受到噪声的影响。尽管如此，仍可以得出结论，从长期数据来看，谷歌趋势数据对价格方向也可在10小时左右以后产生影响，与价格回报预测得到的结论类似。

### Phase 3: Backtesting for Action Recommendation Strategy

The optimal threshold and total return for the baseline model

baseline_simu

The optimal threshold and total return for the best model

action recommendation

最终利用在Phase 2训练最优的LSTM模型对进行比特币交易行为推荐模型，Phase 2发现，当利用短期数据进行模型的构建时，tweets compound score可以显著提高价格移动方向的预测能力，从而最高可获得63%的准确率。利用长期数据进行模型构建时，google trends和 tweets polarity score的组合也可以有效提高预测准确性。利用此两种模型分别构建短期和长期动作推荐模型，并提出比特币交易策略，从而实现特定时间段内的最大利润。其策略已在第三章中提出。构建模拟器对动作推荐进行回测。由于模型预测是通过预测得到的价格方向概率进行推荐，一般情况下阈值为0.5，当价格上升概率大于阈值时会推荐"购买"，通过设置不同阈值可以得到价格回报的百分比。对一个月数据的短期数据进行回测，并通过调整阈值发现获取最大价格回报所需的最佳阈值。如图所示，红色窗口是指购买了比特币的位置，购买后，比特币价格下跌，在随后的一天卖出了股票，此时亏损，即做了错误决策，绿色窗口是它购买了股票，然后比特币价格实际上上涨的地方，此时表示做了正确决策。对0.35到0.65之间的阈值进行测试，定初始阈值为0.5，并测试阈值为0.35和0.65时的价格回报率，选择回报率相比初始阈值上涨的阈值，在其与初始阈值之间进一步选择阈值测试，以此方法逐渐缩小阈值并确定最终阈值。最终发现当阈值为0.41时可得到最大回报。如图所示，假设在当月的第一个小时购买了100美元的比特币，那么在进行了一个月的模拟交易后，总回报率可达21.79%，最终可得到121.79美元。此时的图示中绿色窗口占比较多，证明做了正确决策。

将未添加社交媒体情绪的原始特征训练的LSTM模型作为基线模型，同样为其构建交易行为推荐模型，且为其调整阈值，以确定最大回报率。如图所示，最终确定其最佳阈值为0.52或其以上，此时回报率为0，即既不盈利也不亏损，这表明在当月第一小时买入比特币后，之后一个月一直处于"Wait"动作，而当阈值为0.51时价格开始出现0.67%的亏损。这说明分类模型输出的概率大多处于0.48到0.52之间，表明价格上升和下降的趋势不能被很好得区分。而使用额外特征调整后的模型提高了两类的概率，从而获得更高的回报。

以同样的方法构建长期动作推荐模型并进行测试，最终发现在阈值为0.64时，利用社交媒体数据优化的模型在进行半年的模拟交易后可得到最大的价格回报率27.03%, 而未添加社交媒体数据的基线模型的最佳阈值为0.58，最大的回报率只能达到6.21%。如图所示，在长期的交易中得到了更高的回报，优化的模型在后期价格急剧下降时仍能做出正确决策，然而基线模型在后期则不再能够明显区分价格下降与上升的趋势。进一步验证了Google Trends和tweets 情绪得分推荐模型的优化能力。其他阈值的所获得利润仍可在附录中看到。

> 利用在Phase 2中训练好的最优LSTM模型，构建了比特币交易行为推荐模型。第二阶段发现，在使用短期数据进行模型构建时，推特情绪得分（tweets compound score）能显著提高价格移动方向的预测能力，最高可达到63%的准确率。基于此模型，进一步构建了动作推荐模型，并提出了比特币交易策略，旨在在一个月内获得最大的价格回报。该策略已在第三章中详述。使用模拟器对动作推荐进行了回测。
>
> 由于模型预测基于预测得到的价格方向概率进行推荐，通常将阈值设为0.5。当价格上涨的概率超过该阈值时，推荐进行“购买”操作。通过不同阈值的设置，可以得到不同的价格回报百分比。对一个月的短期数据进行回测，并通过调整阈值，找到了获得最大价格回报所需的最佳阈值。如图所示，红色窗口表示购买比特币的位置，购买后，比特币价格下跌，在随后的一天卖出，导致亏损，即为错误决策。绿色窗口表示购买比特币后，实际上比特币价格上涨，表示为正确决策。
>
> 在0.4到0.6的阈值范围内进行选择，最终发现将阈值设为0.41时，可以获得最大回报。假设初始购买100美元的比特币，经过一个月的模拟交易后，总回报率可达21.23%，最终获得一定金额的回报。在此情况下，图中绿色窗口占比较多，表明做出了正确决策。然而，当阈值在0.43到0.6之间时，回报率为负数，说明出现了开始的亏损。这一结果与第二阶段中模型针对“上涨”和“下跌”两类价格方向的F1-Score相符，其中“下跌”的平均预测准确率要比“上涨”的平均预测准确率高约0.9。因此，这一模拟器的推荐阈值可以确定在0.41左右



综上，这一章对整个研究进行了实施，研究对象为来自公共数据集的社交媒体情绪和谷歌趋势，以及比特币历史每小时价格数据，研究目的是探索两者可能存在的联系，并利用训练好的最佳模型构建动作推荐模型。本章先对初步处理好的社交媒体数据进行了进一步的处理并利用斯皮尔曼相关性和格兰杰因果测试对必要的特征进行了选择，同时提取出了可能对价格回报预测及价格方向预测产生预测能力的特征。之后将数据分为一个月的短期数据和六个月的较长期数据并分别进行研究。通过分析因果关系和预测能力发现，无论是短期还是长期，谷歌趋势均对其价格回报具有一定的滞后影响，从长期来看，基本在2个滞后后产生较大影响。对于价格方向预测，短期内推文情绪得分可对其产生较大影响，平均可将预测准确率提高3\%，最高可提高8\%。但是推文情绪的影响周期较短，基本在2到15个小时间，它在长期上对比特币价格的影响力并不明显，因为社交媒体是在实时变化的，公众的情绪容易变化也易被煽动。谷歌趋势更倾向于在长期数据上展现影响力。尽管如此，利用长期数据进行训练和预测的准确性仍无法提升过多，这可能是因为比特币的高度波动性，其在2022年一月到六月期间价格从四万下降到了两万。最终利用在短期数据上得到的最佳LSTM模型进行action recommendation，构建模拟器进行回测，确定最佳阈值以确保模拟交易能达到最大回报，本模型的最佳阈值为0.41。

> This chapter presents a comprehensive implementation of the research, focusing on social media sentiment, Google Trends data, and historical hourly price data of Bitcoin sourced from public datasets. The objectives of the study involve exploring potential interconnections among these data sets and constructing action recommendation models using optimized LSTM models. Initially, the preprocessed social media data underwent further refinement through Spearman correlation and Granger causality tests to identify pertinent features. Extracted from this process were features that could potentially impact price return and price direction predictions.
>
> With these insights, the data was segregated into short-term data spanning one month and long-term data covering six months, enabling a more detailed examination. By synthesizing causality and predictive potential, it was deduced that Google Trends data exhibited a lagged effect on price returns in both short and long terms, with the effect manifesting around two time units after the initial data point in the long-term dataset. Concerning price direction predictions, tweet sentiment scores exerted significant influence in the short term, resulting in an average accuracy boost of 3 percent, and sometimes even reaching 8 percent. Nonetheless, the influence of tweet sentiment has a shorter duration, primarily spanning from 2 to 15 hours. As such, its impact on Bitcoin prices lacks prominence in the long term, a characteristic attributed to the real-time nature of social media and its susceptibility to abrupt mood shifts.
>
> In contrast, Google Trends data leans towards manifesting influence over extended periods. However, despite training models on long-term data for forecasting, accuracy gains remain constrained. This limitation likely stems from Bitcoin's pronounced volatility, particularly during the period between January and June 2022, when its value plummeted from $40,000 to $20,000.
>
> Ultimately, the optimal LSTM model trained on short-term data was employed to construct the action recommendation model, which was subsequently backtested via simulation. By calibrating thresholds, optimal values were determined to ensure that simulated trades yield maximum returns. In this model, the ideal threshold was set at 0.41. This value was arrived at by considering the classification accuracy disparities between "up" and "down" categories and validated through the simulator.
>
> In conclusion, this study offers a comprehensive exploration encompassing social media sentiment, Google Trends data, and Bitcoin price data. Leveraging deep learning techniques, it delivers a promising Bitcoin trading behavior recommendation model, especially adept at predicting outcomes in short-term data analysis. Furthermore, it sheds light on the significance of distinct features and the influence of volatility on model accuracy across various timeframes, providing invaluable insights for the development of effective trading strategies.




















$$

$$




$$

$$
### Visualization





[^56]: https://pypi.org/project/binance-historical-data/#toc-entry-17
[^58]: https://www.kaggle.com/datasets/leukipp/reddit-crypto-data
[^57]: https://www.kaggle.com/datasets/hiraddolatzadeh/bitcoin-tweets-2021-2022
[^59]: Cryptocurrency Price Prediction Using Tweet Volumes and Sentiment Analysis
[^60]: https://www.wordstream.com/google-trends
[^61]: http://erikjohansson.blogspot.com/2014/12/creating-daily-search-volume-data-from.html
[^62]: https://uk.finance.yahoo.com/

[^63]: https://www.kaggle.com/code/hiraddolatzadeh/01-tweet-cleaning
[^64]: The predictive power of public Twitter sentiment for forecasting cryptocurrency prices
[^65]: Bitcoin Evolution Analytics: Twitter Sentiments to Predict Price Change as Bearish or Bullish
[^66]: Predicting Fluctuations in Cryptocurrency Transactions Based on User Comments and Replies
[^67]: https://sraf.nd.edu/loughranmcdonald-master-dictionary/
[^68]: https://builtin.com/data-science/shapiro-wilk-test
[^70]: https://www.simplilearn.com/tutorials/statistics-tutorial/spearmans-rank-correlation
[^69]: https://statisticsbyjim.com/basics/spearmans-correlation/
[^71]: https://www.analyticsvidhya.com/blog/2021/08/granger-causality-in-time-series-explained-using-chicken-and-egg-problem/
[^72]: https://www.sciencedirect.com/topics/social-sciences/granger-causality-test
[^73]: https://www.machinelearningplus.com/time-series/augmented-dickey-fuller-test/

[^74]: Short Term Prediction on Bitcoin Price Using ARIMA Method
[^75]: https://www.statology.org/rmse-vs-r-squared/
[^76]: Time-series forecasting of Bitcoin prices using high-dimensional features: a machine learning approach
[^77]: https://www.v7labs.com/blog/f1-score-guide
[^78]: Using sentiment analysis on Reddit to predict stock returns
[^79]: Bitcoin price change and trend prediction through twitter sentiment and data volume
[^80]: Predicting tomorrow’s cryptocurrency price using a LSTM model, historical prices and Reddit comments
[^81]: https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MinMaxScaler.html
[^82]: Short-term bitcoin market prediction via machine learning
[^83]: Stock market's price movement prediction with LSTM neural networks
[^84]: Do Google Trends forecast bitcoins? Stylized facts and statistical evidence
[^85]: https://towardsdatascience.com/convolutional-neural-networks-explained-9cc5188c4939
[^86]: Convolution on neural networks for high-frequency trend prediction of cryptocurrency exchange rates using technical indicators
[^87]: Prediction of cryptocurrency returns using machine learning
[^88]: https://www.census.gov/content/dam/Census/library/working-papers/1980/adrm/1980x11arimamanual.pdf
[^89]: https://www.scribbr.com/statistics/akaike-information-criterion/#:~:text=To%20compare%20models%20using%20AIC%2C%20you%20need%20to%20calculate%20the,calculating%20log%2Dlikelihood%20is%20complicated!
[^90]: https://analyticsindiamag.com/quick-way-to-find-p-d-and-q-values-for-arima/

