>作者：连玉君 ([知乎](https://zhuanlan.zhihu.com/arlion) | [简书](http://www.jianshu.com/u/69a30474ef33) | [码云](https://gitee.com/arlionn))    
>
>&emsp;&emsp;[ …… Stata连享会 - 往期精彩推文 ……](https://www.jianshu.com/p/de82fdc2c18a)

----
> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)
---




  

### 背景
交乘项的使用越来与普遍。Stata 官方提供的 `margins` 以及 `marginsplot` 命令可以很好地分析和图示边际效果。其局限在于只能呈现特定数值上的边际效应，而无法连续的呈现边际效应。

外部命令 `interflex` 可能弥补这一局限。

这里介绍的 `marginscontplot2` 也可以很好地实现这一分析目的。

### 命令解释和帮助文件
`marginscontplot2` provides a graph of the marginal effect of a continuous predictor on the response variable in the most recently fitted regression model. See Royston (2013) for details and examples; the paper is available at http://www.stata-journal.com/article.html?article=gr0056.

- `help marginscontplot2` // Graph margins for continuous predictors

### Examples
```stata
*-Basic examples
  sysuse auto, clear
  regress mpg i.foreign weight
  marginscontplot2 weight, name(my_graph)
  marginscontplot2 weight, at1(2000(100)4500) ci
  marginscontplot2 weight foreign, var1(20) at2(0 1)
  marginscontplot2 weight foreign, var1(20) at2(0 1) ///
      ci combopts(ycommon imargin(small))
 
*-Example using a log-transformed covariate
  gen logwt = log(weight)
  regress mpg i.foreign c.logwt i.foreign#c.logwt
  quietly summarize weight
  range w1 r(min) r(max) 20
  generate logw1 = log(w1)
  marginscontplot2 weight (logwt), var1(w1 (logw1)) ci
```
输出效果：
![marginscontplot2 - 输出效果.png](https://upload-images.jianshu.io/upload_images/7692714-a3d83832c97a5784.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### References

- Royston, P. 2013. marginscontplot2: Plotting the marginal effects of continuous predictors.  *Stata Journal*, 13(3): 510-527.


>#### 关于我们
- 【**Stata 连享会(公众号：StataChina)**】由中山大学连玉君老师团队创办，旨在定期与大家分享 Stata 应用的各种经验和技巧。
- 公众号推文同步发布于 [【简书-Stata连享会】](http://www.jianshu.com/u/69a30474ef33) 和 [【知乎-连玉君Stata专栏】](https://www.zhihu.com/people/arlionn)。可以在**简书**和**知乎**中搜索关键词`Stata`或`Stata连享会`后关注我们。
- 推文中的相关数据和程序，以及 [Markdown 格式原文](https://gitee.com/arlionn/jianshu) 可以在 [【Stata连享会-码云】](https://gitee.com/arlionn) 中获取。[【Stata连享会-码云】](https://gitee.com/arlionn) 中还放置了诸多 Stata 资源和程序。如 [Stata命令导航](https://gitee.com/arlionn/stata/wikis/Home) ||  [stata-fundamentals](https://gitee.com/arlionn/stata-fundamentals) ||  [Propensity-score-matching-in-stata](https://gitee.com/arlionn/propensity-score-matching-in-stata) || [Stata-Training](https://gitee.com/arlionn/StataTraining) 等。


>#### 联系我们
- **欢迎赐稿：** 欢迎将您的文章或笔记投稿至`Stata连享会(公众号: StataChina)`，我们会保留您的署名；录用稿件达`五篇`以上，即可**免费**获得 Stata 现场培训 (初级或高级选其一) 资格。
- **意见和资料：** 欢迎您的宝贵意见，您也可以来信索取推文中提及的程序和数据。
- **招募英才：** 欢迎加入我们的团队，一起学习 Stata。合作编辑或撰写稿件五篇以上，即可**免费**获得 Stata 现场培训 (初级或高级选其一) 资格。
- **联系邮件：** StataChina@163.com

>#### 往期精彩推文
[Stata连享会推文列表](https://www.jianshu.com/p/de82fdc2c18a)

> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)


---
![欢迎加入Stata连享会(公众号: StataChina)](http://upload-images.jianshu.io/upload_images/7692714-b62d691d48a0adc3.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "扫码关注 Stata 连享会")
