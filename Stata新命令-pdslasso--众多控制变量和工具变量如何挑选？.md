> Source: [Github - aahr1 / pdslasso](https://github.com/aahr1/pdslasso)

> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)



## Stata package: pdslasso

`pdslasso` and `ivlasso` are routines for estimating structural parameters in linear models with many controls and/or instruments. The routines use methods for estimating sparse high-dimensional models, specifically the lasso (Least Absolute Shrinkage and Selection Operator, [Tibshirani 1996](https://doi.org/10.2307/2346178)) and the square-root-lasso (Belloni et al. [2011](https://doi.org/10.1214/14-AOS1204), [2014](https://doi.org/10.1093/restud/rdt044)).

These estimators are used to select controls (`pdslasso`) and/or instruments (`ivlasso`) from a large set of variables (possibly numbering more than the number of observations), in a setting where the researcher is interested in estimating the causal impact of one or more (possibly endogenous) causal variables of interest.

Two approaches are implemented in `pdslasso` and `ivlasso`:

1.  The *post-double-selection* methodology of Belloni et al. ([2012](https://doi.org/10.3982/ECTA9626), [2013](http://arxiv.org/abs/1201.0220), [2014](http://arxiv.org/abs/1201.0220), [2015](http://www.aeaweb.org/articles.php?doi=10.1257/jep.28.2.29), [2016](https://doi.org/10.1080/07350015.2015.1102733)).
2.  The *post-regularization* methodology of [Chernozhukov, Hansen and Spindler (2015)](https://www.aeaweb.org/articles?id=10.1257/aer.p20151022).

For instrumental variable estimation, `ivlasso implements weak-identification-robust hypothesis tests and confidence sets using the [Chernozhukov et al. (2013)](https://projecteuclid.org/euclid.aos/1387313390) sup-score test.

The implemention of these methods in `pdslasso` and `ivlasso` require the Stata program `rlasso` (available in the separate Stata module [lassopack](https://github.com/aahr1/lassopack)), which provides lasso and square root-lasso estimation with data-driven penalization.

## [](https://github.com/aahr1/pdslasso#installation)Installation

To install the latest version from SSC, type

```source-stata
ssc install lassopack, replace
ssc install pdslasso, replace
```

## [](https://github.com/aahr1/pdslasso#help-files)Help files

For further information on `pdslasso` and `ivlasso`, type

```source-stata
help pdslasso
```

The help files contain more information about the implemented routines and examples.

## [](https://github.com/aahr1/pdslasso#acknowledgements)Acknowledgements

Thanks to Sergio Correia for advice on the use of the FTOOLS package.

## [](https://github.com/aahr1/pdslasso#citation)Citation

`pdslasso` and `ivlasso` are not official Stata commands. They are free contributions to the research community, like a paper.
Please cite it as such:

Ahrens, A., Hansen, C.B., Schaffer, M.E. 2018\. pdslasso and ivlasso: Progams for post-selection and post-regularization OLS or IV estimation and inference. [http://ideas.repec.org/c/boc/bocode/s458459.html](http://ideas.repec.org/c/boc/bocode/s458459.html)

## [](https://github.com/aahr1/pdslasso#authors)Authors

Achim Ahrens, Economic and Social Research Institute, Ireland

Christian B. Hansen, University of Chicago, USA

Mark E Schaffer, Heriot-Watt University, UK

## [](https://github.com/aahr1/pdslasso#issues-and-questions)Issues and questions

If you have encountered any issues with **pdslasso**, contact achim.ahrens(at)esri.ie and m.e.schaffer(at)hw.ac.uk. If you have questions about the use of **pdslasso**, contact us via [Statalist](https://www.statalist.org/).



>#### 关于我们

- 【**Stata 连享会(公众号：StataChina)**】由中山大学连玉君老师团队创办，旨在定期与大家分享 Stata 应用的各种经验和技巧。
- 公众号推文同步发布于 [【简书-Stata连享会】](https://www.jianshu.com/u/69a30474ef33) 和 [【知乎-连玉君Stata专栏】](https://zhuanlan.zhihu.com/arlion)。可以在**简书**和**知乎**中搜索关键词`Stata`或`Stata连享会`后关注我们。
- 点击推文底部【阅读原文】可以查看推文中的链接并下载相关资料。
> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)


>#### 联系我们

- **欢迎赐稿：** 欢迎将您的文章或笔记投稿至`Stata连享会(公众号: StataChina)`，我们会保留您的署名；录用稿件达`五篇`以上，即可**免费**获得 Stata 现场培训 (初级或高级选其一) 资格。
- **意见和资料：** 欢迎您的宝贵意见，您也可以来信索取推文中提及的程序和数据。
- **招募英才：** 欢迎加入我们的团队，一起学习 Stata。合作编辑或撰写稿件五篇以上，即可**免费**获得 Stata 现场培训 (初级或高级选其一) 资格。
- **联系邮件：** StataChina@163.com

>#### 往期精彩推文
[Stata连享会推文列表](https://www.jianshu.com/p/de82fdc2c18a)
> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)

---
![欢迎加入Stata连享会(公众号: StataChina)](http://upload-images.jianshu.io/upload_images/7692714-c8acdc4d609d8ef2.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "扫码关注 Stata 连享会")
