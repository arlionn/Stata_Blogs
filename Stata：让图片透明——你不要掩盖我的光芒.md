> Stata连享会  ([知乎](https://zhuanlan.zhihu.com/arlion) | [简书](http://www.jianshu.com/u/69a30474ef33) | [码云](https://gitee.com/arlionn))

> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)



**Source:** [Stata: Transparency in Graphs](http://www.psychstatistics.com/2017/11/23/stata-graph_transparency/)

Stata 15 提供了新功能，可以设定图片的透明度，在多个图片重叠出现时非常实用。

举个例子，假设你想绘制两个正态分布的密度函数曲线图，如果不设定透明度，则图形显示如下，
```stata
twoway function y = normalden(x), range(-4 4) ///
		color(eltgreen) recast(area) ///
	|| function y = normalden(x+.5), range(-4 4) ///
		color(ebblue) recast(area) ///	
		scheme(burd) legend(off)
```
![overlap](http://upload-images.jianshu.io/upload_images/7692714-ea856055da40a2e3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
这里我们使用的选项  `scheme(burd)`，以便将绘图模板设定为 [burd](https://github.com/briatte/burd/wiki)。当然，大家也可以使用 Stata 默认的绘图模板（只需去掉选项 `scheme(burd) ` 即可），这里之所以采用这种模板形式，是为了使图片呈现的效果更佳。如果想安装 [burd](https://github.com/briatte/burd/wiki) 模板，可以使用如下命令：
```stata
ssc install scheme-burd, replace
```
显然，上例中的蓝色分布曲线完全覆盖了下方浅绿色曲线的大部分面积。为了得到更好的图片呈现效果，我们可以设定颜色的透明度，这可以通过选项 `color(ebblue%40)` 来实现。
```stata
twoway function y = normalden(x), range(-4 4) ///
		color(eltgreen) recast(area) ///
	|| function y = normalden(x+.5), range(-4 4) ///
		color(ebblue%40) recast(area) ///	
		scheme(burd) legend(off)
```

![Overlap with Transparency](http://upload-images.jianshu.io/upload_images/7692714-6748577421fc8d4d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 更多链接：[Stata Tutorials](http://www.psychstatistics.com/tutorials/)



>#### 关于我们

- 【**Stata 连享会(公众号：StataChina)**】由中山大学连玉君老师团队创办，旨在定期与大家分享 Stata 应用的各种经验和技巧。
- 公众号推文同步发布于 [【简书-Stata连享会】](https://www.jianshu.com/u/69a30474ef33) 和 [【知乎-连玉君Stata专栏】](https://zhuanlan.zhihu.com/arlion)。可以在**简书**和**知乎**中搜索关键词`Stata`或`Stata连享会`后关注我们。
- 点击推文底部【阅读原文】可以查看推文中的链接并下载相关资料。
- [Stata 现场培训报名中](https://www.jianshu.com/p/af6fb0448297)

>#### 联系我们

- **欢迎赐稿：** 欢迎将您的文章或笔记投稿至`Stata连享会(公众号: StataChina)`，我们会保留您的署名；录用稿件达`五篇`以上，即可**免费**获得 Stata 现场培训 (初级或高级选其一) 资格。
- **意见和资料：** 欢迎您的宝贵意见，您也可以来信索取推文中提及的程序和数据。
- **招募英才：** 欢迎加入我们的团队，一起学习 Stata。合作编辑或撰写稿件五篇以上，即可**免费**获得 Stata 现场培训 (初级或高级选其一) 资格。
- **联系邮件：** StataChina@163.com

>#### 往期精彩推文
[Stata连享会推文列表](https://www.jianshu.com/p/de82fdc2c18a)

> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)




---
![欢迎加入Stata连享会(公众号: StataChina)](http://upload-images.jianshu.io/upload_images/7692714-d3428d352307a5b4.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "扫码关注 Stata 连享会")
