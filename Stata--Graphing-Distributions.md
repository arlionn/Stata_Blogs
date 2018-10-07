
> Source: http://www.psychstatistics.com/2010/11/24/stata-graphing-distributions/

> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)


24 Nov 2010Tags: Stata and Tutorial

# Graphing Distributions

This post will demonstrate how:

1.  Use the `twoway function’ plotting command to visualize distributions
2.  Add colored shading to a graph to visualize portions of a distribution

## The `twoway function` command

The `twoway function` plotting command is used to plot functions, such as `y = mx + b`. If we want to plot the density of a normal distribution across a range of x values, we type `y=normalden(x)`. You can also include graphing options available to twoway plots (e.g., `xtitle`).

```stata
twoway function y=normalden(x), range(-4 4) xtitle("{it: x}") ///
ytitle("Density") title("Standard Normal Distribution")

```

![Stand_Normal.jpg](http://upload-images.jianshu.io/upload_images/7692714-cd50f20cf4cec792.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## Add Shading to a Figure

Suppose we want to shade parts of a distribution above (or below) a particular critical value. For example, we can shade a normal distribution above 1.96 and below -1.96 if we want critical values for a two-tailed test with an alpha-level of .05\. To do this we will draw 3 graphs.

1.  A normal curve from -4 to -1.96
2.  A normal curve from -1.96 to 1.96
3.  A normal curve from 1.96 to 4

The choice of -4 and 4 as upper and lower bounds is arbitrary. You can connect the three graphs by using a double pipe, `||`, between calls to the `twoway function` command. We will shade the area under the curve for #1 and #3 using the `recast(area)` option of `twoway function`. We will assign the color of the shading to dark navy blue using the `color(dknavy)` option. We will leave the area under the curve for #2 unshaded.

```stata
twoway function y=normalden(x), range(-1.96 1.96) color(dknavy) || ///
function y=normalden(x), range(-4 -1.96) recast(area) color(dknavy) || ///
function y=normalden(x), range(1.96 4) recast(area) color(dknavy) ///
xtitle("{it: x}") ///
ytitle("Density") title("Critial Values for Standard Normal") ///
subtitle("Two-tailed test and {&alpha}=.05") ///
legend(off) xlabel(-1.96 0 1.96)

```

![twotail_normal.jpg](http://upload-images.jianshu.io/upload_images/7692714-9f14fd585ba919d8.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

We can repeat for a one-tailed test.

```stata
twoway function y=normalden(x), range(-4 1.64) color(dknavy) || ///
function y=normalden(x), range(1.64 4) recast(area) color(dknavy) ///
xtitle("{it: x}") ///
ytitle("Density") title("Critial Values for Standard Normal") ///
subtitle("One-tailed test and {&alpha}=.05") ///
legend(off) xlabel(0 1.64)

```

![onetailed_normal.jpg](http://upload-images.jianshu.io/upload_images/7692714-0c0ea21d12b9881c.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

We can also visualize other distributions available in Stata. Below, I provide an example of a *t*-distribution with 20 degrees of freedom

```stata
twoway function y=tden(20,x), range(-2.09 2.09) color(dknavy) || ///
function y=tden(20,x), range(-4 -2.09) recast(area) color(dknavy) || ///
function y=tden(20,x), range(2.09 4) recast(area) color(dknavy) ///
xtitle("{it: x}") ///
ytitle("Density") title("Critial Values for {it: t}-distribution with 20 df") ///
subtitle("Two-tailed test and {&alpha}=.05") ///
legend(off) xlabel(-2.09 0 2.09)

```

![tdist20df.jpg](http://upload-images.jianshu.io/upload_images/7692714-02821c353713e2ff.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


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
![欢迎加入Stata连享会(公众号: StataChina)](http://upload-images.jianshu.io/upload_images/7692714-ecac797648ea6c50.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "扫码关注 Stata 连享会")
