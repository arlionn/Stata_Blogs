

> #### [连享会：内生性问题及估计方法专题](https://mp.weixin.qq.com/s/FWpF5tE68lAtCEvYUOnNgw)
[![连享会-内生性专题现场班-2019.11.14-17](https://upload-images.jianshu.io/upload_images/7692714-e53913b2144fcc7c?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](https://gitee.com/arlionn/Course/blob/master/2019Spatial.md)




&emsp;

> 作者：亢延锟 (中央财经大学)      
>     &emsp;     
> Stata 连享会： [知乎](https://zhuanlan.zhihu.com/arlion) | [简书](http://www.jianshu.com/u/69a30474ef33) | [码云](https://gitee.com/arlionn) | [CSDN](https://blog.csdn.net/arlionn)


  
[![点击查看完整推文列表](https://upload-images.jianshu.io/upload_images/7692714-b3f42e5e8d01fe96?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](https://mp.weixin.qq.com/s/RwkuPpLS7bI5C5OhRqjkOw)
> Stata连享会 &emsp; [计量专题](https://gitee.com/arlionn/Course/blob/master/README.md)  || [精品课程](https://mp.weixin.qq.com/s/hWtncj56PeFNL4yg2-va0Q) || [简书推文](https://www.jianshu.com/p/de82fdc2c18a)  || [公众号合集](https://mp.weixin.qq.com/s/RwkuPpLS7bI5C5OhRqjkOw)

@[toc]
## 1. 简介
谢谦等 (2019) 对目前学术界断点回归 (regression-discontinuity designs) 应用的最新进展做了详细的综述，但他们侧重于强调在五大期刊中出现的应用，对还未在五大上出现的多配置变量 RDD (RDD with assignment variables)、分位数 RDD 、拐点回归设计 (regression kink designs)、多断点RDD (RDD with multiple cutoffs)、远离断点处的处理效应的识别方法( methods for extrapolation away from the cutoff)、离散型配置变量 RDD 等新进展未做涉及。

这篇推文主要结合几篇文章对多配置变量 RDD 、多断点 RDD 和远离断点处的处理效应这几种情况的核心思想和文章的主要结论做一大体介绍。值得一提的是，assignment variable 又叫 running variable 和 forcing variable，中文被翻译成配置变量、分配变量、驱动变量、分组变量等不同叫法。在国内大家的使用也各有不同，张川川和陈斌开 (2015) 使用特征变量或驱动变量，邹红和喻开志 (2015) 、雷晓燕等 (2010) 、张川川等 (2014) 也使用驱动变量的说法，黄新飞等 (2014) 和李宏斌等 (2014) 使用运行变量的叫法，秦学征等 (2018) 使用指派变量，刘生龙等 (2016) 使用设计变量的翻译，但是其含义都是一样。

## 2. 允许有多个分配变量的 RDD
Wong et al.  (2013)  介绍了一个多配置变量的断点回归 (multivariate regression-discontinuity design) ，即可以允许有多个配置变量和断点。文章中以有两个配置变量的 MRDD 为例，指出临界平均处理效应 (frontier average treatment effect) 可以被分解成两个单变量 RDD 效应的加权平均，并介绍了边界法 (frontier approach) 、中心化方法 (centering approach) 、单变量方法 (univariate approach) 和工具变量方法 (IV approach) 四种估计方法及其优劣。

举个例子，假设学校要开设一个补习班，如果学校要求阅读成绩低于 60 分的同学参加，那么我们可以用传统的断点回归来评估补习班的效果。如果学校要求阅读成绩和数学成绩有任何一个低于 60 分，都要参加补习班。这时如果还需要评估补习班的作用，传统的断点回归方法就不适用了，而要使用上文提出的多多配置变量的断点回归 (MRDD) 。

> 图 1  
 ![Figure 1](https://upload-images.jianshu.io/upload_images/7692714-e3cf93d57797f0d6?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如上图所示，T1 部分是阅读成绩不达标的同学，T3 是数学成绩不达标的同学，T2 是都不达标的同学。在这种情况下，会有三个处理效应，首先根据传统 RD 的思路，会在阅读成绩和数学成绩 60 分处形成两个特定边界效应 (frontier-specific effect) ，另外一个是边界平均处理效应 (frontier average treatment effect) ，即总体的平均效应，用潜在因果框架可以写成：

$$\tau_{\mathrm{MRD}}=E\left[Y_{i}(1)-Y_{i}(0) |\left(R_{i}, M_{i}\right) \in F\right] 
\quad (1)$$

Wong et al.  (2013)  指出，边界平均处理效应可以被分解成两个特定边界效应的加权平均值，即：

$$\begin{aligned} 
\tau_{\mathrm{MRD}} 
&=E\left[G_{i} |\left(R_{i}, M_{i}\right) \in F\right] \\ 
&=w_{R} E\left[G_{i} | R_{i} \in F_{R}\right]+w_{M} E\left[G_{i} | M_{i} \in F_{M}\right] \\ 
&=w_{R} \tau_{R}+w_{M} \tau_{M} 
\end{aligned}  
\quad (2)
$$

正因为如此，MRDD 要求在两个独立的断点处，满足传统 RD 的所有假设，同时，MRDD 估计的有效性还高度依赖于配置变量的度量和标准差。例如，当两个配置变量是收入和年龄时，当收入的单位由万元改为元时，收入处的处理效应在平均处理效应中的权重也会改变，从而使 MRDD 的平均处理效应的估计发生改变，类似的，对配置变量的极端值的处理也会产生相同的影响。

对于 MRDD 的四种估计方法 Frontier Approach，Centering Approach ，Univariate Approach 和 IV Approach，这四种方法的做法各异，依赖的前提假设也不同，限于篇幅原因，感兴趣的同学可以详细阅读文章的第三部分。在这里根据文章对四种方法的优劣做简单的总结。

Frontier Approach 的优点在于可以同时估计所有的处理效应 ($\tau_{\mathrm{MRD}}$, $\tau_{M}$,和 $\tau_{R}$)，因此可以识别异质性处理效应，同时比 Centering Approach更有效率，即拥有更小的标准误。缺点在于依赖于对响应面和核密度进行正确估计的强假设，并且进行数值积分的非参数估计十分繁琐。

Centering Approach 的优点在于使用相对容易的方法处理具有许多配置变量的 MRDD ，因为这一方法的本质是一个降维的过程。缺点在于降维的过程掩盖了处理效应的异质性，并且这一过程依赖于较为复杂的方程形式来刻画配置变量和结果变量之间的关系，不如 Frontier Approach 有效。

Univariate Approach 的优点在于可以直接计算和发现处理效应的异质性，比 IV 方法更有效率，即拥有更小的标准误。缺点是不能计算 $\tau_{\mathrm{MRD}}$。IV Approach 的优点尚不明确，缺点是尚未有实证研究证明这一方法的有效性，同时也比 Univariate Approach 的效率更低。

接下来做一个简单总结：

1.	对于 MRDD 的经济意义需要研究者谨慎考虑，在许多情况下，边界平均治疗效果可能没有一个有意义的解释。如果在一个边界，估计表明没有影响，在另一个边界，显示出有显著的积极影响，那么的平均影响取决于一个与单位和度量相关的加权方案。

2.	如果没有相当强的假设，MRDD 中 $\tau_{M}$ 和 $\tau_{R}$ 的估计一般性要远小于传统的断点回归，同时在 MRDD 中估计的仍然是一个局部平均处理效应 (LATE) 。

3.	作者不建议使用 IV 进行估计，虽然在满足分析假设的情况下，它会产生无偏估计，但文章的模拟结果表明，与其他三种方法相比，IV 方法降低了统计精度，同时在文章中模拟环境中，IV方法也没有展现出其他一些比较优势。

&emsp;
> #### [连享会计量方法专题……](https://gitee.com/arlionn/Course/blob/master/README.md)

## 3. 允许有多个拐点的 RDD
在传统的断点回归设计中，研究者往往根据断点来估计 LATE 来进行因果效应的识别，但是在实际情况中断点的数量可能并不唯一，比如要研究国家贫困县补贴的影响，但是不同省份贫困县的标准不一样，在使用 RD 时便会出现多个断点的情况，此时可以对其进行标准化之后在进行传统 RD 的操作，但是这种处理方式的具体形式和经济意义仍然值得商榷，并且研究者同样失去了对不同断点异质性的考察。

利用 Cattaneo et al. (2016)  文中的一个例子，在一个两党制的选举中，50% 的得票率自然而然可以被当作胜选的断点，但是在有三个或三个以上政党参与的选举中，可能胜选的政党得票率不超过40%也是极有可能的，这种情况在在政治学研究中普遍存在，这也是促使作者探究允许多个断点存在的rd的主要原因。

Cattaneo et al. (2016) 指出，在一系列严格的假设下，这种将多个断点标准化的做法所得到的效应仍然是多个断点效应的加权平均值，并且权重取决于在各断点附近样本观察值的多少。作者在文中指出，允许多个断点 RDD 的估计方法在一定的假设下可以改写成上文介绍的多个配置变量的  RDD, 并在文中介绍了相应的估计方法。

作者在文中还给予研究者一些对允许多个断点 RDD 的使用建议，作者认为应该先画图，看看样本分布图是否很直观的存在多个断点，如果分布中大量样本集中于一个断点，也可以视同使用单个断点进行传统的断点回归，当确实存在多个断点时，研究者可以有以下几个处理方法：

1. 可以使用单个断点的估计方法，直接忽略不同断点之间的异质性问题，或者假设每个断点的效应都是一样的，当然这需要结合理论和具体问题进行论述。

2. 研究者可以承认异质性的存在，但声明我们关注的重点是平均处理效应，异质性问题并不重要。

3. 可以先用单个断点的回归方法，再将另外一部分集中于第二个断点附近的样本剔除，如果回归结果不发生大的变化，则有理由相信这样的处理是合理的。

如果最后要使用多个断点的 RDD，则必须要确定断点是否是累积的，如果断点是累积的，那么每一个样本面临的断点是配置变量的一个确定的函数，那么这就使得断点回归中样本分组不能是内生的假设无法满足。

&emsp;
> #### [连享会计量方法专题……](https://mp.weixin.qq.com/s/hWtncj56PeFNL4yg2-va0Q)


## 4. 远离断点处的处理效应
我们知道，RD 估计的是一个局部平均处理效应 (LATE)，这仅对断点两侧的样本有意义，离断点较远的样本实际上并不参与处理效应的识别。有时候 RD 估计的 LATE 本身是重要的，例如该不该给数学和阅读不及格的孩子上补习课，但是有时候这个 LATE 对于政策执行者的意义仍然不足。比如这一结果不能说明是否对所有孩子都上补习班也有相同的效果，这就是一个政策是否可以推广的问题。可惜传统的 RD 并不能回答这一问题。

Cattaneo et al. (2018) 在一个一个允许多个断点回归的框架下构造了一个处理效应的合理外推，这一思路借鉴了双重差分方法的思路，核心是解决潜在因果识别中的数据缺失问题。

> 图 2 
![Figure 2](https://upload-images.jianshu.io/upload_images/7692714-aa9f49300d39e57d?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如图 2 所示，一个拥有两个断点的 RDD, 一部分样本在 l 处断开，一部分样本在 h 处断开，此时根据上文的介绍，我们可以得到 $l$ 和 $h$ 处的处理效应 (frontier-specific effect) $\tau_{\ell}(\ell)$ 和 $\tau_{h}(h)$ ，这两个处理效应仍然是一个靠近 $l$ 和 $h$ 处的 LATE。一个问题是，如果我们想将这一处理效应合理外推至 $\overline{x}$ 处，即我们想估计 $\tau_{\ell}(\overline{x})$，该如何实现呢？

> 图 3    
 ![Figure 3](https://upload-images.jianshu.io/upload_images/7692714-5dd7a62442109b3e?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


看到 **图 3**， 熟悉双重差分方法的读者可以很容易理解作者外推的思路。作者需要估计 $\tau_{\ell}(\overline{x})$ ，必须要得到 $a$ 点和 $b$ 点的数值，很显然b点是观测不到的，也就是反事实的。因为这部分样本中凡是大于 $l$ 的样本都进行了处理，因此要想将处理效应外推至 $\overline{x}$ 处，必须要解决的是 $b$ 点数据缺失的问题。

从 **图 3** 中可以看到，我们可以观测的点是 $a$、$c$、$d$ 和 $e$。其中，$d$ 和 $e$ 是在 $h$ 处断开的另一部分样本。很显然，如果 $B(\ell)=B(\overline{x})$，那么 $a$ 和 $b$ 之间的距离，也就是 $\tau_{\ell}(\overline{x})$ ，可以被写作：

$$\begin{aligned} \overline{a c}-\overline{e d} &=\left\{\mu_{1, \ell}(\overline{x})-\mu_{0, \ell}(\overline{x})\right\}-\left\{\mu_{0, \ell}(\ell)-\mu_{0, \hbar}(\ell)\right\} \\ &=\left\{\tau_{\ell}(\overline{x})+B(\overline{x})\right\}-\{B(\ell)\} \\ &=\tau_{\ell}(\overline{x}) \end{aligned}  \tag{3}$$

此时，在估计 $\tau_{\ell}(\overline{x})$ ，所有的点都是可观测的。但是同时，正如双重差分方法一样，这种外推同样严格依赖于共同趋势的假设，即在不同断点处的样本组，拥有共同的发展趋势。

&emsp;

## 5. 参考文献
  - Wong, V. C., P. M. Steiner, T. D. Cook, 2013, Analyzing regression-discontinuity designs with multiple assignment variables:A comparative study of four estimation methods, Journal of Educational and Behavioral Statistics, 38 (2): 107-141. [[PDF]](https://journals.sagepub.com/doi/abs/10.3102/1076998611432172)
  - Cattaneo, M. D., L. Keele, R. Titiunik, G. Vazquez-Bare, 2016, Interpreting regression discontinuity designs with multiple cutoffs, The Journal of Politics, 78 (4): 1229-1248. [[PDF1]](https://www.journals.uchicago.edu/doi/pdfplus/10.1086/686802)，[[Supplemental Material]](https://www.journals.uchicago.edu/doi/suppl/10.1086/686802/suppl_file/150592Appendix.pdf)，[[论文重现资料]](https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/6N2PN4)
  - Cattaneo, M. D., L. Keele, R. Titiunik, G. Vazquez-Bare, 2018, Extrapolating treatment effects in multi-cutoff regression discontinuity designs, arXiv preprint arXiv:1808.04416.，[[PDF]](https://arxiv.org/pdf/1808.04416.pdf)。   
  - 谢谦, 薛仙玲, 付明卫. 断点回归设计方法应用的研究综述[J]. 经济与管理评论, 2019, 35(02):  69-79. 
  - 刘生龙, 周绍杰, 胡鞍钢. 义务教育法与中国城镇教育回报率: 基于断点回归设计[J]. 经济研究, 2016, 51(02): 154-167. 
  - 邹红, 喻开志. 退休与城镇家庭消费: 基于断点回归设计的经验证据[J]. 经济研究, 2015, 50(01): 124-139. 
  - 黄新飞, 陈珊珊, 李腾. 价格差异、市场分割与边界效应——基于长三角15个城市的实证研究[J]. 经济研究, 2014, 49(12): 18-32. 
  - 张川川, 陈斌开. 社会养老能否替代家庭养老? ——来自中国新型农村社会养老保险的证据[J]. 经济研究, 2014, 49(11): 102-115. 
  - 秦雪征, 庄晨, 杨汝岱. 计划生育对子女教育水平的影响——来自中国的微观证据[J]. 经济学(季刊), 2018, 17(03): 897-922. 
  - 张川川, John Giles, 赵耀辉. 新型农村社会养老保险政策效果评估——收入、贫困、消费、主观福利和劳动供给[J]. 经济学(季刊), 2015, 14(01): 203-230. 
  - 李宏彬, 施新政, 吴斌珍. 中国居民退休前后的消费行为研究[J]. 经济学(季刊), 2015, 14(01): 117-134. 
  - 雷晓燕, 谭力, 赵耀辉. 退休会影响健康吗? [J]. 经济学(季刊), 2010, 9(04): 1539-1558. 

>#### 关于我们

- **「Stata 连享会」** 由中山大学连玉君老师团队创办，定期分享实证分析经验， 公众号：**StataChina**。
- 公众号推文同步发布于 [CSDN](https://blog.csdn.net/arlionn) 、[简书](http://www.jianshu.com/u/69a30474ef33) 和 [知乎Stata专栏](https://www.zhihu.com/people/arlionn)。可在百度中搜索关键词 「Stata连享会」查看往期推文。
- 点击推文底部【阅读原文】可以查看推文中的链接并下载相关资料。
- **欢迎赐稿：** 欢迎赐稿。录用稿件达 **三篇** 以上，即可 **免费** 获得一期 Stata 现场培训资格。
- **E-mail：** StataChina@163.com
- 往期推文：[计量专题](https://gitee.com/arlionn/Course/blob/master/README.md)  || [精品课程](https://mp.weixin.qq.com/s/hWtncj56PeFNL4yg2-va0Q) || [简书推文](https://www.jianshu.com/p/de82fdc2c18a)  || [公众号合集](https://mp.weixin.qq.com/s/AWxDPvTuIrBdf6TMUzAhWw) 

[![点击此处-查看完整推文列表](https://upload-images.jianshu.io/upload_images/7692714-1658eefb1ca51924?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](https://mp.weixin.qq.com/s/RwkuPpLS7bI5C5OhRqjkOw)


---
![欢迎加入Stata连享会(公众号: StataChina)](https://upload-images.jianshu.io/upload_images/7692714-60f7db1d9a858926.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "扫码关注 Stata 连享会")



