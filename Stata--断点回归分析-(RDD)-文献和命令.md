> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)



### Overview

There are several review articles on RD methodology and empirical practice. This course will discuss, and update as appropriate, topics in [Imbens and Lemieux (2008)](https://scholar.harvard.edu/files/imbens/files/regression_discontinuity_designs_a_guide_to_practice.pdf?m=1360042039) and [Lee and Lemieux (2010)](https://core.ac.uk/download/pdf/6684346.pdf). Since these reviews do not cover many of the most recent methodological results available in the literature, we are currently working on an up-to-date review on RD methodology (Cattaneo and Titiunik, 2016), which will be the main focus of this workshop. See also Skovron and Titiunik (2015) for a recent, updated review with particular focus on Political Science.

### RDD 分析的 Stata 命令

- `rdrobust`: RD inference employing local polynomial and partitioning methods. See Calonico, Cattaneo, and Titiunik (2014a, 2015b) for introductions, and Calonico, Cattaneo, Farrell, and Titiunik (2016b) for the most recent upgrades.
- `rddensity`: RD density test for manipulation testing. See Cattaneo, Jansson, and Ma (2016b) for an introduction.
- `rdlocrand`: RD inference employing randomization inference methods. See Cattaneo, Titiunik, and Vazquez-Bare (2016b) for an introduction

- Source：[Cattaneo-2018-Handbook](http://www-personal.umich.edu/~titiunik/books/CattaneoIdroboTitiunik2018-Cambridge-Vol1.pdf)

#### RDD 简明教程 (适于上手操作)
- Jacob, R., P. Zhu, M. A. Somers, H. Bloom, 2012, A practical guide to regression discontinuity, Mdrc, [- PDF -](https://www.mdrc.org/sites/default/files/RDD%20Guide_Full%20rev%202016_0.pdf)， 模型设定部分的解释非常清晰，基本上没有用复杂的数学公式。

#### RDD 的适用条件？
- Schochet, P., Cook, T., Deke, J., Imbens, G., Lockwood, J.R., Porter, J., Smith, J. (2010). Standards for Regression Discontinuity Designs. [-PDF-](http://ies.ed.gov/ncee/wwc/pdf/wwc_rd.pdf)

#### RDD 文献清单
- Matias D. Cattaneo - Syllabus - 2016, Short Course in Regression Discontinuity Designs, [- PDF -](https://econ.georgetown.edu/sites/econ/files/documents/rdcourse-cattaneo-gcep-apr2016_0.pdf), 给出了一份较为完整的 RDD 参考文献清单。

### RDD 相关 Stata 命令的介绍性文档

- Cattaneo and Titiunik (2018): Regression Discontinuity Designs: A Review, coming soon.
- Cattaneo, Idrobo and Titiunik (2018): [A Practical Introduction to Regression Discontinuity Designs: Volume I](http://www.umich.edu/~cattaneo/books/Cattaneo-Idrobo-Titiunik_2018_Cambridge-Vol1.pdf). *Cambridge Elements: Quantitative and Computational Methods for Social Science*, Cambridge University Press. [Replication Files](https://sites.google.com/site/rdpackages/replication/cit-2018-cambridge)

- Cattaneo, Idrobo and Titiunik (2018): [A Practical Introduction to Regression Discontinuity Designs: Volume II](http://www.umich.edu/~cattaneo/books/Cattaneo-Idrobo-Titiunik_2018_Cambridge-Vol2.pdf). *Cambridge Elements: Quantitative and Computational Methods for Social Science*, Cambridge University Press. [Replication Files](https://sites.google.com/site/rdpackages/replication/cit-2018-cambridge)

- Cattaneo, Titiunik and Vazquez-Bare (2017): [Comparing Inference Approaches for RD Designs: A Reexamination of the Effect of Head Start on Child Mortality](https://sites.google.com/site/rdpackages/rdlocrand/Cattaneo-Titiunik-VazquezBare_2017_JPAM.pdf?attredirects=0). *Journal of Policy Analysis and Management* 36(3): 643-681, Summer 2017.
[Replication Files](https://sites.google.com/site/rdpackages/replication/ctv-2017-jpam)

- Cattaneo and Vazquez-Bare (2016): [The Choice of Neighborhood in Regression Discontinuity Designs](https://sites.google.com/site/rdpackages/Cattaneo-VazquezBare_2016_ObsStud.pdf?attredirects=0).
*Observational Studies* 2: 134-146, December 2016.



### 参考文献
- Calonico, S., M. D. Cattaneo, and M. H. Farrell (2016): “On the Effect of Bias Estimation on Coverage Accuracy in Nonparametric Inference,” working paper, University of Michigan.
- Calonico, S., M. D. Cattaneo, M. H. Farrell, and R. Titiunik (2016a): “Regression Discontinuity Designs Using Covariates,” working paper, University of Michigan.
- Calonico, S., M. D. Cattaneo, M. H. Farrell, and R. Titiunik (2016b): “rdrobust: Software for Regression Discontinuity Designs,” working paper, University of Michigan.
- Calonico, S., M. D. Cattaneo, and R. Titiunik (2014a): “Robust Data-Driven Inference in the Regression-Discontinuity Design,” Stata Journal, 14(4), 909–946.
- Calonico, S., M. D. Cattaneo, and R. Titiunik (2014b): “Robust Nonparametric Confidence Intervals for Regression-Discontinuity Designs,” Econometrica, 82(6), 2295–2326.
- Calonico, S., M. D. Cattaneo, and R. Titiunik (2015a): “Optimal Data-Driven Regression Discontinuity Plots,” Journal of the American Statistical Association, 110(512), 1753–1769.
- Calonico, S., M. D. Cattaneo, and R. Titiunik (2015b): “rdrobust: An R Package for Robust Nonparametric Inference in RegressionDiscontinuity Designs,” R Journal, 7(1), 38–51.
- Card, D., D. S. Lee, Z. Pei, and A. Weber (2015): “Inference on Causal Effects in a Generalized Regression Kink Design,” Econometrica, 83(6), 2453–2483.
- Cattaneo, M. D., N. Idrobo, and R. Titiunik (2018a): A Practical Introduction to Regression Discontinuity Designs: Part I. In preparation for Cambridge Elements: Quantitative and Computational Methods for Social Science, Cambridge University Press. [- PDF -](http://www-personal.umich.edu/~titiunik/books/CattaneoIdroboTitiunik2018-Cambridge-Vol1.pdf)
- Cattaneo, M. D., N. Idrobo, and R. Titiunik (2018b): A Practical Introduction to Regression Discontinuity Designs: Part II. In preparation for Cambridge Elements: Quantitative and Computational Methods for Social Science, Cambridge University Press. [- PDF -](http://www-personal.umich.edu/~titiunik/books/CattaneoIdroboTitiunik2018-Cambridge-Vol2.pdf)
- Cattaneo, M. D., B. Frandsen, and R. Titiunik (2015): “Randomization Inference in the Regression Discontinuity Design: An Application to Party Advantages in the U.S. Senate,” Journal of Causal Inference, 3(1), 1–24.
- Cattaneo, M. D., M. Jansson, and X. Ma (2016a): “Simple Local Regression Distribution Estimators with an Application to Manipulation Testing,” working paper, University of Michigan.
- Cattaneo, M. D., M. Jansson, and X. Ma (2016b): “rddensity: Manipulation Testing based on Density Discontinuity,” working paper, University of Michigan.
- Cattaneo, M. D., L. Keele, R. Titiunik, and G. Vazquez-Bare (2016): “Interpreting Regression Discontinuity Designs with Multiple Cutoffs,” Journal of Politics, forthcoming.
- Cattaneo, M. D., and R. Titiunik (2016): “Regression Discontinuity Designs: A Review of Recent Methodological Developments,” manuscript in preparation, University of Michigan.
- Cattaneo, M. D., R. Titiunik, and G. Vazquez-Bare (2016a): “Comparing Inference Approaches for RD Designs: A Reexamination of the Effect of Head Start on Child Mortality,” working paper, University of Michigan.
- Cattaneo, M. D., R. Titiunik, and G. Vazquez-Bare (2016b): “rdlocrand: Inference in Regression Discontinuity Designs under Local Randomization,” Stata Journal, forthcoming.
- Hahn, J., P. Todd, and W. van der Klaauw (2001): “Identification and Estimation of Treatment Effects with a Regression-Discontinuity Design,” Econometrica, 69(1), 201–209.
- Imbens, G., and T. Lemieux (2008): “Regression Discontinuity Designs: A Guide to Practice,” Journal of Econometrics, 142(2), 615–635.
- Imbens, G. W., and K. Kalyanaraman (2012): “Optimal Bandwidth Choice for the Regression Discontinuity Estimator,” Review of Economic Studies, 79(3), 933–959.
- Keele, L. J., and R. Titiunik (2015): “Geographic Boundaries as Regression Discontinuities,” Political Analysis, 23(1), 127–155.
- Lee, D. S. (2008): “Randomized Experiments from Non-random Selection in U.S. House Elections,” Journal of Econometrics, 142(2), 675–697.
- Lee, D. S., and T. Lemieux (2010): “Regression Discontinuity Designs in Economics,” Journal of Economic Literature, 48(2), 281–355.
- Lee, D. S., and T. Lemieux 2014, Chapter 14: Regression discontinuity designs in social sciences[C], in H. Best,C. Wolf eds, The sage handbook of regression analysis and causal inference. [PDF](http://econ.sites.olt.ubc.ca/files/2014/02/Lee-Lemieux-rev.pdf)
- McCrary, J. (2008): “Manipulation of the running variable in the regression discontinuity design: A density test,” Journal of Econometrics, 142(2), 698–714.
- Porter, J. (2003): “Estimation in the Regression Discontinuity Model,” working paper, University of Wisconsin.
- Skovron, C., and R. Titiunik (2015): “A Practical Guide to Regression Discontinuity Designs in Political Science, with,” working paper, University of Michigan.
- Wooldridge, J. (2010): Econometric Analysis of Cross-Section and Panel Data. MIT Press, Cambridge, MA.



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
>[Stata连享会推文列表](https://www.jianshu.com/p/de82fdc2c18a)
>
> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)


---
![欢迎加入Stata连享会(公众号: StataChina)](http://upload-images.jianshu.io/upload_images/7692714-4765fb4a9787321f.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "扫码关注 Stata 连享会")
