> 作者：连玉君 ([知乎](https://zhuanlan.zhihu.com/arlion) | [简书](http://www.jianshu.com/u/69a30474ef33) | [码云](https://gitee.com/arlionn))

> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)


**事件研究法 (Event Study)** 由 Ball& Brown (1968) 以及 Fama et al. (1969) 开创。
其原理是根据研究目的选择某一特定事件，研究事件发生前后样本股票收益率的变化，进而解释特定事件对样本股票价格变化与收益率的影响，主要被用于检验事件发生前后价格变化或价格对披露信息的反应程度。

事件研究法以有效市场假说为基础，即股票价格反映所有已知的公共信息，由于投资者是理性的，投资者对新信息的反应也是理性的。

因此，在样本股票实际收益中剔除假定某个事件没有发生而估计出来的正常收益 (normal return) 就可以得到异常收益 (abnormal return)，异常收益可以衡量股价对事件发生或信息披露异常反应的程度。

### 事件研究法原理

- [事件研究法概览 - eventstudymetrics.com](https://eventstudymetrics.com/index.php/event-studies/)
- [Eventus-Guide-8-Public.pdf](http://www.eventstudy.com/Eventus-Guide-8-Public.pdf) Eventus 软件的说明书，附录部分可参考
- [Event Studies: Assessing the Market Impact of Corporate Policy (PDF)](http://www1.american.edu/academic.depts/ksb/finance_realestate/rhauswald/fin673/673mat/Hauswald%20%282002%29,%20Event%20Studies%20Note%20%28FIN-02-005%29.pdf)
- [Lecture Notes on Event Study Analysis Jin-Lung Lin (PDF)](http://faculty.ndhu.edu.tw/~jlin/files/EventStudy.pdf)
- [经典论文 - MacKinlay (1997), Event Studies in Economics and Finance.pdf](http://www1.american.edu/academic.depts/ksb/finance_realestate/rhauswald/fin673/673mat/MacKinlay%20%281997%29,%20Event%20Studies%20in%20Economics%20and%20Finance.pdf)


### 事件研究法资源
- 一网打尽事件研究法 [【EventStudyTools】](https://www.eventstudytools.com/)
- [参数和非参数检验，正常回报的估计等 eventstudymetrics.com](https://eventstudymetrics.com/index.php/event-study-methodology/)
- [MIT Event Study Webpage](http://web.mit.edu/doncram/www/eventstudy.html) 提供了大量有关 Event Study 的参考文献和数据来源说明

### Stata 范例
- [Princeton Stata 范例](http://dss.princeton.edu/online_help/stats_packages/stata/eventstudy.html) 解读了 Event Study 中最关键的编程问题
- [StackOverFlow `atuo.dta` 模拟 Stata 范例](https://stackoverflow.com/questions/41622943/stata-event-study-graph-code)
- [StackOverFlow Event Study 问题汇总](https://stackoverflow.com/search?q=Event+Study)
- [GitHub - Stata Event Study](https://github.com/arlionn/Stata-Event_Study) 提供了一些自己编写的命令(Fama-Macbech 1973，Rolling beta 等)

### Stata 外部命令

使用这些命令，可以一次性完成基本的 Event Study 估计和检验工作，非常便捷。  
需要事先用 `ssc install cmdname, replace` 下载最新版；亦可使用 `findit` 命令搜索相应的命令，以便查看完整的 Stata 范例数据和 dofile。 

- `help eventstudy` //基本命令
- `help eventstudy2` //更为丰富的选项和检验统计量， [PDF说明文档](https://www.researchgate.net/publication/320452443_Running_event_studies_using_Stata_the_estudy_command)
- `help estudy`  // Stata Journal 18-2，只能指定单一的事件日期，但可以分析特定公司的事件效果

>#### 关于我们
- 【**Stata 连享会(公众号：StataChina)**】由中山大学连玉君老师团队创办，旨在定期与大家分享 Stata 应用的各种经验和技巧。
- 公众号推文同步发布于 [【简书-Stata连享会】](http://www.jianshu.com/u/69a30474ef33) 和 [【知乎-连玉君Stata专栏】](https://www.zhihu.com/people/arlionn)。可以在**简书**和**知乎**中搜索关键词`Stata`或`Stata连享会`后关注我们。
- 推文中的相关数据和程序，以及 [Markdown 格式原文](https://gitee.com/arlionn/jianshu) 可以在 [【Stata连享会-码云】](https://gitee.com/arlionn) 中获取。[【Stata连享会-码云】](https://gitee.com/arlionn) 中还放置了诸多 Stata 资源和程序。如 [Stata命令导航](https://gitee.com/arlionn/stata/wikis/Home) ||  [stata-fundamentals](https://gitee.com/arlionn/stata-fundamentals) ||  [Propensity-score-matching-in-stata](https://gitee.com/arlionn/propensity-score-matching-in-stata) || [Stata-Training](https://gitee.com/arlionn/StataTraining) 等。


>#### 联系我们
- **欢迎赐稿：** 欢迎将您的文章或笔记投稿至`Stata连享会(公众号: StataChina)`，我们会保留您的署名；录用稿件达`五篇`以上，即可**免费**获得 Stata 现场培训 (初级或高级选其一) 资格。
- **意见和资料：** 欢迎您的宝贵意见，您也可以来信索取推文中提及的程序和数据。
- **招募英才：** 欢迎加入我们的团队，一起学习 Stata。合作编辑或撰写稿件五篇以上，即可**免费**获得 Stata 现场培训 (初级或高级选其一) 资格。
- **联系邮件：** StataChina@163.com


> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)


---
![欢迎加入Stata连享会(公众号: StataChina)](http://upload-images.jianshu.io/upload_images/7692714-2f355ade6fac4292..jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "扫码关注 Stata 连享会")
